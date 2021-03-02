# Objectives
Deploy the Follower to your EKS Cluster Node

### 1.0. Collect those yaml files
authn-k8s-cluster.yaml

### 2.0. Push Conjur Contrainer Image to AWS Container Repositories Services

1. Remark: If you want to know more about ECR https://www.youtube.com/watch?v=Yy9AGt4m0_I
2. Login te ECR
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
3. Tag and push the conjur contrainer image to AWS ECR
   ```
   docker tag registry.tld/conjur-appliance:12.0.0 <your aws ecr account><your name>/conjur-appliance:12.0.0
   docker push <your aws ecr account><your name>/conjur-appliance:12.0.0
   ```
   ```
   Example
   docker tag registry.tld/conjur-appliance:12.0.0 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/conjur-appliance:12.0.0
   docker push 409556437035.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/conjur-appliance:12.0.0
   The push refers to repository [1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/conjur-appliance] 4df15f8657f2: Pushed
   12.0.0: digest: sha256:0b9aba05256abc17a70aca36e4911d13b4bbecfef861c56e8d0d9b08a4c3ed2e size: 530
   ```

### 3.0. Create Follower namespace and service account
1. Log in to the Jump Host
   ```bash
   kubectl create namespace dap
   ```
2. Create service account for conjur follower deployment 
   ```bash
   kubectl create serviceaccount conjur-cluster -n dap
   ```

