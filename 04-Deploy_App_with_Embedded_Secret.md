# Objectives
In this lab, we are going to get familar with EKS by deploying a sample application.
We will deploy our test application 'cityapp'. 
The application connect to mysql database at Jump Host and display random city name. 
The application will use hardcoded credential in the deployment configuration.

### 1.0. Collect cityapp images and cityapp-hardcode.yaml

### 2.0. Load Cityapp container images to Jump Host Docker

1. Log in to your Jump Host in AWS
2. Load the cityapp container images to docker
   ```
   cd ~
   mkdir ./cityapp/images
   cd ./cityapp/images
   *** Reamrk: copy Cityapp container image to ~/cityapp/images
   sudo docker load -i cityapp.tar.gz

### 3.0. Push Cityapp Contrainer Image to AWS Container Repositories Services
1. Remark: If you want to know more about ECR https://www.youtube.com/watch?v=Yy9AGt4m0_I
   ```
   docker tag cityapp:1.0 <your aws account ID>.dkr.ecr.ap-southeast-1.amazonaws.com/<your name>/cityapp:1.0
   docker push <your aws account ID>.dkr.ecr.ap-southeast-1.amazonaws.com/<your name>/cityapp:1.0
   ```
   ```
   Example
   docker tag cityapp:1.0 123456789000.dkr.ecr.ap-southeast-1.amazonaws.com/ivanleecityapp:1.0
   docker push 123456789000.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/cityapp:1.0
   ```
   
### 4.0. Create Cityapp Namespace in EKS Cluster

1. Let's create a project called `cityapp`
```
oc new-project cityapp
oc project
```

## Push image
7.	Push cityapp image from local Docker to OpenShift integrated registry. 
This image is already pre-built and load to local registry on `docker03` as `cityapp:1.0`

To login to OpenShift integrated registry, we can use token from oc whoami -t
```
oc whoami -t
docker login -u _ -p [Enter token from previous step] docker-registry-default.apps.okd.cyberarkdemo.com
```

To do this in single command
```
docker login -u _ -p $(oc whoami -t) docker-registry-default.apps.okd.cyberarkdemo.com
```

Tag and push cityapp container image to Openshift registry
```
docker tag cityapp:1.0 docker-registry-default.apps.okd.cyberarkdemo.com/cityapp/cityapp
docker push  docker-registry-default.apps.okd.cyberarkdemo.com/cityapp/cityapp
```

## Deploy app

Review cityapp-hardcode.yaml deployment config template and make necessary change. 
:bangbang:	You need to change something here

```bash
cat cityapp-hardcode.yaml
```

Then apply this file to Openshift
```
oc apply -f cityapp-hardcode.yaml
```

> :bulb:	Take note that docker registry in deployment config file is referencing internal svc url. 
> This internal registry usually does not require DockerPullSecret to fetch image from within cluster.
> Openshift integrated registry **external** URL: `docker-registry-default.apps.okd.cyberarkdemo.com`
> Openshift integrated registry **internal** URL (service name inside cluster: `docker-registry.default.svc:5000 OR docker-registry.default.svc.cluster.local:5000`

10.	Check Pods and test that the application is working
```
oc get pods
oc exec -it [pod-name] curl http://localhost:3000
```

:question:	**Question: Where's the risk?***

## Create secret 

Now we will modify this deployment to store DBPassword using OpenShift Secret

1. Create an openshift OpenShift to store mysql01 password.
   Replace XXXXX with password that CPM changed.   Get the latest version from PVWA.
```
oc create secret generic mysql01-secret --from-literal=password=XXXXXXXXX
```
3. Browse to `https://okd.cyberarkdemo.com:8443/console`

4. Log in as `admin`

5.	Go to `Environment` tab under  application deployment at `cityapp > Applications > Deploymnet

6. Remove `DBPassword` 

7. Click `Add Vaule from Config Map or Secret` with name `DBPassword` and choose `mysql01-secret` as resource and `password` as key

7. Click `Save`.   A notification will be shown
   
 
6.	Let's confirm that application still function with the new pods from updated deployment
```
oc get pods
oc exec -it [pod-name] curl http://localhost:3000
```

## Create Route for remote access

1.	Create route for this application in OpenShift UI to expose this application to outside cluster at `cityapp > Services > cityapp-hardcode > Create route`

2. Leave all the default setting and click `Create`. 

3.	A url will be created under `Hostname`, should be something like `http://cityapp-hardcode-cityapp.apps.okd.cyberarkdemo.com`

4. Try accessing this link from `CLIENT` VM

## Extra Challenges

This section is for quick learner who has completed earlier than expected.   Very well done!

 - What is project, serviceaccount, route, pods, container, deploymentconfig in OpenShift? (Hint: https://docs.openshift.com/container-platform/3.11/welcome/index.html)
 
 - Where're embedded secrets?  (Hint: You can find them in files & web UI)
 
 - What will happen to the application if we trigger CPM to rotate password?
 

