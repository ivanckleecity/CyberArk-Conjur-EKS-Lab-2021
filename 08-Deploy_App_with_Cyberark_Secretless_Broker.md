# Objectives
We are going to deploy secure applications with Secretless Broker. For you to better understand how CyberArk Secretless Broker, plesae take 15 mins to read [Secretless Broker online doc](https://secretless.io/) before you start this lab session

### 1.0. Collect those yaml files
- cityapp-secretless.yaml
- secretless.yaml

### 1.0 These should have been done in the previous lab. Make sure all are in place. 
- Follower certificate in configmap
- Conjur policy for cityapp-secretless
- EKS clusterrolebinding to allow follower to validate Pods in cityapp namespace

### 3.0 Review and make necessary changes Secretless configuration file `secretless.yaml` and load it as a new configmap
1. Create folder /home/ec2-user/cityapp/cityapp_secretless
2. Copy cityapp-secretless.yaml and secretless.yaml to /home/ec2-user/cityapp/cityapp_secretless
```
cd /home/ec2-user/cityapp/cityapp_secretless
kubectl create configmap cityapp-secretless-config --from-file=secretless.yaml -n cityapp
```

### 4.0 Review cityapp-secretless.yaml deployment file and modify it with proper value for your environment. 
```
cd /home/ec2-user/cityapp/cityapp_secretless
kubectl apply -f cityapp-secretless.yaml -n cityapp
```

### 5.0 Check if your cityapp summon container running good
1. Go in your cityapp-summon-init shell
```bash
kubectl get pod -n cityapp
```
3. Use curl to check if the Random City name can show up
```
kubectl exec -it cityapp-secretless-<your pod ID> -n cityapp bash
curl -k http://127.0.0.1:3000
```
5. There is the example output
```
[ec2-user@dap lab5_secretless]$ kubectl get pod -n cityapp
NAME                                   READY   STATUS    RESTARTS   AGE
cityapp-secretless-e7559d50-4p4xx     2/2     Running   0          9h
[ec2-user@dap lab5_secretless]$ kubectl exec -it cityapp-secretless-e7559d50-4p4xx -n cityapp bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl kubectl exec [POD] -- [COMMAND] instead.
Defaulting container name to cityapp.
Use 'kubectl describe pod/cityapp-secretless-e7559d50-4p4xx -n cityapp' to see all of the containers in this pod.
bash-4.4# curl http://127.0.0.1:3000
<title> Random World Cities! </title>
<br><br>
<p style="font-size:30px"><b>Shagamu</b> is a city in Ogun, Nigeria with a population of 117200
<br><br><br><p>
<small>Connected to database world on 127.0.0.1:3306 using username:  and password: </small>
bash-4.4#
```

