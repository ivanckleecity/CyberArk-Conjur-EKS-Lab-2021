# Objectives
We are going to deploy secure applications with summon injects secrets into environment variables. We will now redeploy cityapp from Task 4 without hardcode credential by using summon to retrieve secrets from Conjur and inject to environment variable. To use summon to inject secret for container, the summon binary has to be included in application image.  The cityapp is already built with summon. You may review the Dockerfile for more detail

### 1.0. Collect those yaml files
- projects-authn.yaml
- app-identity.yaml
- safe-permission.yaml
- secrets.yaml
- follower-dap.cer.pem

### 2.0 Then load these policy files to permit application namespace to authenticate and fetch secrets.
1. Copy projects-authn.yaml, app-identity.yaml and safe-permission.yaml to /home/ec2-user/conjur_policy
2. Load the below policy to Conjur Master
```bash
conjur policy load root /root/conjur_policy/projects-authn.yaml
conjur policy load root /root/conjur_policy/app-identity.yaml
conjur policy load root /root/conjur_policy/safe-permission.yaml
```
### 3.0 Add the User name and password to cust_portal/username and cust_portal/password
```bash
conjur variable values add cust_portal/username cityapp
conjur variable values add cust_portal/password Cyberark1
```
### 4.0 Create configmap for follower certificate in Cityapp namespace
```bash
kubectl create configmap follower-certificate --from-file=ssl-certificate=<(cat follower-dap.cer.pem) -n cityapp
```
- the follower-dap.cer.pem are mentioned in step 1 

### 5.0 Load the secrets yaml file as EKS Cluster ConfigMap
```bash
kubectl create configmap cityapp-summon-init-config --from-file=secrets.yaml -n cityapp
```
- the secrets.yaml are mentioned in step 1 

### Deploy the Cityapp Summon to your EKS Cluster
1. Review and make necessary changes to cityapp-summon-init.yaml deployment file
2. Apply this to deploy cityapp-summon-init Observe how summon is triggered in command overrides. 
```bash
kubectl apply -f cityapp-summon-init.yaml -n cityapp
```

### Check if your cityapp summon container running good
1. Go in your cityapp-summon-init shell
```bash
kubectl get pod -n cityapp
```
3. Use curl to check if the Random City name can show up
```
kubectl exec -it cityapp-summon-init-<your pod ID> -n cityapp bash
curl -k http://127.0.0.1:3000
```
5. There is the example output
```
[ec2-user@dap ~]$ kubectl get pod -n cityapp
NAME                                   READY   STATUS    RESTARTS   AGE
cityapp-summon-init-65f49fc8b7-k55xh   1/1     Running   0          9h

[ec2-user@dap ~]$ kubectl exec -it cityapp-summon-init-65f49fc8b7-k55xh -n cityapp bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl kubectl exec [POD] -- [COMMAND] instead.
bash-4.4# curl -k http://127.0.0.1:3000
<title> Random World Cities! </title>
<br><br>
<p style="font-size:30px"><b>Kallithea</b> is a city in Attika, Greece with a population of 114233
<br><br><br><p>
<small>Connected to database world on exampledb.cusiivm6n9hm.ap-southeast-1.rds.amazonaws.com:3306 using username: admin and password: Ixia1234!</small>

```
