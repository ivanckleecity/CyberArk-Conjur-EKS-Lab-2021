# Objectives
Deploy Conjur Follower to your EKS Cluster Node. We will deploy Conjur followers with seed-fetcher, which automatically authenticate and retrieve seed on Pod start up. This allow self-healing and auto scaling of follower pods. We will also enable k8s authenicator to support the following labs.

### 1.0. Push Conjur Contrainer Image to AWS Container Repositories Services

1. If you want to know more about ECR https://www.youtube.com/watch?v=Yy9AGt4m0_I
2. :bangbang: Reminder: You have legal responsibly to keep the CyberArk Conjur Image/Installer for your own use only, PLEASE MAKE SURE all CyberArk Conjur Image in AWS ECR is set in "Private" to avoid unexpected illegal distribution of DAP images
3. Login te ECR
   ```bash
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <your aws ecr region account dns>
   ```
   ```
- Example output:
   ```
   aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com
   WARNING! Your password will be stored unencrypted in /home/ec2-user/.docker/config.json.
   Configure a credential helper to remove this warning. See
   https://docs.docker.com/engine/reference/commandline/login/#credentials-store

   Login Succeeded
   ```
3. Tag and push the conjur contrainer image to AWS ECR
   ```
   sudo docker tag registry.tld/conjur-appliance:12.0.0 <your aws ecr account><your name>/conjur-appliance:12.0.0
   sudo docker tag cyberark/dap-seedfetcher:0.1.5 <your aws ecr account><your name>Don/dap-seedfetcher:0.1.5
   sudo docker push <your aws ecr account><your name>/conjur-appliance:12.0.0
   sudo docker push <your aws ecr account><your name>/dap-seedfetcher:0.1.5
   ```
   ```
- Example:
   ```
   docker tag registry.tld/conjur-appliance:12.0.0 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/conjur-appliance:12.0.0
   docker push 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/conjur-appliance:12.0.0
   
   docker tag cyberark/dap-seedfetcher:0.1.5 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/dap-seedfetcher:0.1.5
   docker push 1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/dap-seedfetcher:0.1.5
   
   The push refers to repository [1234567890123.dkr.ecr.ap-southeast-1.amazonaws.com/ivanlee/conjur-appliance] 4df15f8657f2: Pushed
   12.0.0: digest: sha256:0b9aba05256abc17a70aca36e4911d13b4bbecfef861c56e8d0d9b08a4c3ed2e size: 530
   ```

### 2.0. Create Follower namespace and service account
1. Log in to the Jump Host
   ```bash
   kubectl create namespace dap
   ```
2. Create service account for conjur follower deployment 
   ```bash
   kubectl create serviceaccount conjur-cluster -n dap
   ```
   
### 3.0. Load Conjur Policies
1. Download authn-k8s-cluster.yaml. Please review conjur policy files authn-k8s-cluster.yaml and make necessary changes if your envirunment is difference to this lab before load it to conjur
```bash
cd ~/conjur_policy
wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task06/authn-k8s-cluster.yaml
```
2. Load the authn-k8s-cluster.yaml to Conjur Master
   ```bash
   conjur policy load root /root/conjur_policy/authn-k8s-cluster.yaml
   ```
3. Remark, If an error bash: conjur: command not found..., please execute: alias conjur='docker run --rm -it --network host -v $HOME:/root -it cyberark/conjur-cli:5'

### 4.0. Initialize internal CA
1. Initialize internal CA that will be used for K8S Authenticator
   ```bash
   sudo docker exec conjur-appliance chpst -u conjur conjur-plugin-service possum rake authn_k8s:ca_init["conjur/authn-k8s/okd"]
   ```
2. Access Conjur master UI and verify that conjur/authn-k8s/okd/ca/cert and conjur/authn-k8s/okd/ca/key have value not blank

### 5.0. Enable K8S Authetnication on Master node
1. Add CONJUR_AUTHENTICATORS="authn,authn-k8s/okd" to /opt/conjur/etc/conjur.conf in Master container
   ```bash
   sudo docker exec -it conjur-appliance vi /opt/conjur/etc/conjur.conf
   ```
2. Restart conjur service to apply this change.
   ```bash
   sudo docker exec conjur-appliance sv restart conjur
   ```
3. Verify that okd authenticator is now enabled on Master
   ```bash
   curl -k https://master-dap.cyberarkdemo.com/info
   ```
4. You see this output
    ```
     "enabled": [
      "authn",
      "authn-k8s/okd"
    ```
    
### 6.0. Create cluster role and role binding for conjur-cluster service account.
1. Download conjur-authenticator-role.yaml, conjur-authenticator-role-binding.yaml, conjur-authenticator-clusterole-binding.yaml. Please review those role binding files and make necessary changes if your envirunment is difference to this lab before load it to EKS
```bash
mkdir -p ~/conjur_follower
cd ~/conjur_follower
wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task06/conjur-authenticator-clusterole-binding.yaml
wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task06/conjur-authenticator-role-binding.yaml
wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task06/conjur-authenticator-role.yaml
```
5. Use kubectl to apply it.
   ```bash
   kubectl apply -f ./conjur-authenticator-role.yaml
   kubectl apply -f ./conjur-authenticator-role-binding.yaml
   kubectl apply -f ./conjur-authenticator-clusterole-binding.yaml -n dap
   ```
   
### 7.0. Configure EKS Cluster API Detail in Conjur
```bash
TOKEN_SECRET_NAME="$(kubectl get secrets -n dap \
| grep 'conjur.*service-account-token' \
| head -n1 \
| awk '{print $1}')"
CA_CERT="$(kubectl get secret -n dap $TOKEN_SECRET_NAME -o json \
| jq -r '.data["ca.crt"]' \
| base64 --decode)"
SERVICE_ACCOUNT_TOKEN="$(kubectl get secret -n dap $TOKEN_SECRET_NAME -o json \
| jq -r .data.token \
| base64 --decode)"
API_URL="$(kubectl config view --minify -o json \
| jq -r '.clusters[0].cluster.server')"
```

### 8.0. Verify the vaules in the environmental variables
```bash
echo $TOKEN_SECRET_NAME
echo $CA_CERT
echo $SERVICE_ACCOUNT_TOKEN
echo $API_URL
```
- Remark: If you missed any output from the above echo, please check and try to do it

### 9.0. Load them to Conjur as variables
```bash
conjur variable values add conjur/authn-k8s/okd/kubernetes/ca-cert "$CA_CERT"
conjur variable values add conjur/authn-k8s/okd/kubernetes/service-account-token "$SERVICE_ACCOUNT_TOKEN"
conjur variable values add conjur/authn-k8s/okd/kubernetes/api-url "$API_URL"
```

### 10.0. Add Conjur Master certificate to config map. This will be used by seedfetcher to validate Conjur Master
```bash
openssl s_client -showcerts -connect master-dap.cyberarkdemo.com:443 -servername master-dap.cyberarkdemo.com </dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > master-certificate.pem
kubectl create configmap master-certificate --from-file=ssl-certificate=<(cat master-certificate.pem) -n dap
```

### 11.0. Deploy the Follower Seedfetcher
1. Download follower-dap-seedfetcher.yaml. Please review and make necessary changes to follower-dap-seedfetcher.yaml, if your envirunment is difference to the lab
```bash
cd ~/conjur_follower
wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task06/follower-dap-with-seedfetcher.yaml
```
2. Use kubectl command to apply.
```bash
kubectl apply -f ./follower-dap-with-seedfetcher.yaml -n dap
```

### 12.0 Check if your follow running correctly
1. Check if your follower pod status is showing "Running"
```bash
kubectl get pod -n dap
```
2. Sample Output
```
[ec2-user@ip-10-0-1-54 conjur_follower]$ kubectl get pod -n dap
NAME                        READY   STATUS    RESTARTS   AGE
follower-7f67ddf796-r8m2z   1/1     Running   0          6m21s
```
