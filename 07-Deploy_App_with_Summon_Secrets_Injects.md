# Objectives
Deploy secure applications with summon injects secrets into environment variables

### 1.0. Collect the Conjur Installer and seed fectcher from your local CyberArk Sales Engineer Team


conjur policy load root /root/conjur_policy/projects-authn.yaml
conjur policy load root /root/conjur_policy/app-identity.yaml
conjur policy load root /root/conjur_policy/safe-permission.yaml
conjur variable values add cust_portal/username cityapp
conjur variable values add cust_portal/password Cyberark1
kubectl create configmap follower-certificate --from-file=ssl-certificate=<(cat follower-dap.cer.pem) -n cityapp
kubectl create configmap cityapp-summon-init-config --from-file=secrets.yaml -n cityapp


# Secure applications with init Container and summon 


We will now redeploy cityapp from Lab 1 without hardcode credential by using summon to retrieve secrets from Conjur and inject to environment variable. To use summon to inject secret for container, the summon binary has to be included in application image.  The cityapp is already built with summon. You may review the Dockerfile for more detail


1. Login to `DAP-MASTER` as `root`

1. Push kubernetes authentication image to OpenShift registry

```bash
docker tag cyberark/conjur-kubernetes-authenticator:latest docker-registry-default.apps.okd.cyberarkdemo.com/cityapp/conjur-kubernetes-authenticator
docker push  docker-registry-default.apps.okd.cyberarkdemo.com/cityapp/conjur-kubernetes-authenticator
```

2. Review the policy files for application and make necessary changes (e.g. safe detail in safe-permission.yaml file). 

![policy](./images/04-policy.png)

:bulb:	Try comparing the above with the list of `Groups` in DAP


3. Then load these policy files to permit application namespace to authenticate and fetch secrets.

 
```bash
conjur policy load root /root/policy/projects-authn.yaml
conjur policy load root /root/policy/app-identity.yaml
conjur policy load root /root/policy/safe-permission.yaml
```


4.	Before proceed, make sure that we are on cityapp namespac
```
oc project cityapp
```

5.	 The conjur-cluster service account should already have clusterrolebinding from previous lab. 
     If not, you will need to load rolebinding for this cityapp namespace.
  
6.	Obtain follower certificate and create configmap for this one

```bash
cd /root/lab4_summon
openssl s_client -showcerts -connect follower-dap.apps.okd.cyberarkdemo.com:443 -servername follower-dap.apps.okd.cyberarkdemo.com </dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > follower-certificate.pem
oc create configmap follower-certificate --from-file=ssl-certificate=<(cat follower-certificate.pem)
```

7.	Now we will create config file for summon. Review `secrets.yaml` file and make necessary changes. 

:bulb:	Try comparing the above with the list of `Secrets` in DAP

8. Load this file as Openshift ConfigMap .
```bash
oc create configmap cityapp-summon-init-config --from-file=secrets.yaml
```

8.	Review and make necessary changes to `cityapp-summon-init.yaml` deployment file and then apply this to deploy `cityapp-summon-init` Observe how summon is triggered in command overrides.

```bash
oc apply -f cityapp-summon-init.yaml
```

9.	Verify that the cityapp is running. You can test access from chrome browser to route created for this deployment. 

:bulb: Refer to previous lab on how to locate the created route

    
 ## Extra Tech Challenge

 - What do you find in Conjur audit log?
 - Init container will be removed after completing the execution.   How can we troubleshoot?
 - How to switch the above init container to sidecar?
 - How to clean up this lab in order to try again?
