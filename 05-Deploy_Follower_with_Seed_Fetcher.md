# Objectives
Deploy the Follower to your EKS Cluster Node

### 1.0. Collect those yaml files
authn-k8s-cluster.yaml

### 2.0. Push and Tag Conjur Contrainer Image to AWS Container Repositories Services
1. Login te ECR
   ```
      aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <your aws ecr region account dns>
   ```
   ```
   Example
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com
   WARNING! Your password will be stored unencrypted in /home/ec2-user/.docker/config.json.
   Configure a credential helper to remove this warning. See
   https://docs.docker.com/engine/reference/commandline/login/#credentials-store

   Login Succeeded
```


Remark: If you want to know more about ECR https://www.youtube.com/watch?v=Yy9AGt4m0_I

### 3.0. Create Follower namespace and service account
1. Log in to the Jump Host
   ```bash
   kubectl create namespace dap
   ```
2. Create service account for conjur follower deployment 
   ```bash
   kubectl create serviceaccount conjur-cluster -n dap
   ```

https://www.youtube.com/watch?v=Yy9AGt4m0_I
