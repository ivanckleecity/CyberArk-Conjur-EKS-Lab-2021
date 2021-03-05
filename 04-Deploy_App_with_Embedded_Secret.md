# Objectives
In this lab, we are going to get familar with EKS by deploying a sample application. We will deploy our test application 'cityapp' to your EKS Cluster. The application "Cityapp" will access resouce from your mysql database in Jump Host where you created in task 04. The Cityapp will be function as display random city name. In this task, Cityapp will use hardcoded credential in the deployment configuration.

### 1.0. Collect cityapp images and cityapp-hardcode.yaml

### 2.0. Load Cityapp container images to Jump Host Docker

1. Log in to your Jump Host in AWS
2. Load the cityapp container images to docker
   ```
   cd ~
   mkdir -p ./cityapp/images
   cd ./cityapp/images
   wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task04/build.tar.gz
   tar -xvf ./build.tar.gz
   cd ./build
   ./build.sh
3. Check if your cityapp container images was builded and pushed into your local docker repository
   ```
   docker images
   
   Output Example
   [ec2-user@ip-10-0-1-54 build]$ docker images
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   cityapp             1.0                 5c126890f139        2 minutes ago       291MB
   ```

### 3.0. Push Cityapp Contrainer Image to AWS Elastic Container Registry
1. Remark: If you want to know more about ECR https://www.youtube.com/watch?v=Yy9AGt4m0_I
   ```
   docker tag cityapp:1.0 <your aws account ID>.dkr.ecr.ap-southeast-1.amazonaws.com/<your name>/cityapp:1.0
   docker push <your aws account ID>.dkr.ecr.ap-southeast-1.amazonaws.com/<your name>/cityapp:1.0
   ```
   ```
   Example
   docker tag cityapp:1.0 123456789000.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/cityapp:1.0
   docker push 123456789000.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/cityapp:1.0
   ```
   
### 4.0. Create Cityapp Namespace in EKS Cluster

1. Let's create a namespace called `cityapp`
   ```
   kubectl create namespace cityapp
   ```
### 5.0. Deploy cityapp to EKS Cluster

```
cd ~
mkdir -p ~/cityapp/cityapp_hardcode
Remark: copy cityapp-hardcode.yaml to this folder
cd ~/cityapp/cityapp_hardcode
kubectl delete -f cityapp-hardcode.yaml -n cityapp
```
```
Output Example
$ kubectl apply -f cityapp-hardcode.yaml
serviceaccount/cityapp-hardcode created
service/cityapp-hardcode created
deployment.apps/cityapp-hardcode created
```
### 6.0. Check if your cityapp embedded Secret running good

1. Check if your cityapp pod running with out errors
```
kubectl get pod -n cityapp
```
```
Output Example
NAME                                READY   STATUS    RESTARTS   AGE
cityapp-hardcode-5fc4d759c6-thrrp   1/1     Running   0          14s
```
```
2. Go in cityapp shell to run curl to check if your EKS cluster can access the MySQL
kubectl exec -it cityapp-hardcode-<pod id> -n cityapp bash
curl http://127.0.0.1:3000
```
```
Example
$ kubectl exec -it cityapp-hardcode-5fc4d759c6-thrrp -n cityapp bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.

bash-4.4# curl http://127.0.0.1:3000
<title> Random World Cities! </title>
<br><br>
<p style="font-size:30px"><b>Sheffield</b> is a city in England, United Kingdom with a population of 431607
<br><br><br><p>
<small>Connected to database world on mysql01.cyberarkdemo.com:3306 using username: cityapp and password: Cyberark1</small>

bash-4.4# curl http://127.0.0.1:3000
<title> Random World Cities! </title>
<br><br>
<p style="font-size:30px"><b>Marrakech</b> is a city in Marrakech-Tensift-Al, Morocco with a population of 621914
<br><br><br><p>
<small>Connected to database world on mysql01.cyberarkdemo.com:3306 using username: cityapp and password: Cyberark1</small>
bash-4.4#
```

### 7.0. Congratulation!!!
If you can see random city and courty name, that mean your EKS cluster and mysql World DataBase are running good
