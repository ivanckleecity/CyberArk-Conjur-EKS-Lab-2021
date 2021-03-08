# Objectives
Install CyberArk Conjur Master in Jump Host

### 1.0. Collect the Conjur Installer and seed fectcher from your local CyberArk Sales Engineer Team
1. conjur-appliance_xx.x.x.tar.gz
2. dap-seedfetcher_x.x.x.tar.gz (Remark: the xx.x.x is the Conjur version)
- This lab guide is based on Conjur 12.0 version
   
### 1.1. Install Conjur Master

1. Login to your Jump Host
2. Load the Conjur and seed fetcher containers to your Docker
   ```
   mkdir -p ~/conjur_installer
   ```
   - Upload conjur-appliance_xx.x.x.tar.gz and dap-seedfetcher_x.x.x.tar.gz to ~\conjur_installer
   ```bash
   cd ~/conjur_installer
   sudo docker load -i conjur-appliance-12.0.0.tar.gz
   sudo docker load -i dap-seedfetcher_0.1.5.tar.gz
   sudo docker tag registry.tld/conjur-appliance:12.0.0 conjur-appliance:12.0.0
   ```
3. Check your images
   ```bash
   docker images
   ```
   
   Example output
   ```bash
   REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
   conjur-appliance                12.0.0              6e8aad127725        2 months ago        1.19GB
   registry.tld/conjur-appliance   12.0.0              6e8aad127725        2 months ago        1.19GB
   cyberark/dap-seedfetcher        0.1.5               fed063656f4b        8 months ago        30MB
   ```

### 1.2. Start Conjur Master with signed certificate

1. Spin up the master container
   ```bash
   sudo docker run --name conjur-appliance -d --restart=always --security-opt seccomp:unconfined -p "443:443" -p "636:636" -p "5432:5432" -p "1999:1999" conjur-appliance:12.0.0
   ```

2. Generate Conjur Certificate Certificate
   
   - For this lab, we generated a set of Certificate already.
   - If you want to generate your own Certificate, please follow this Conjur Cert generation guide. https://github.com/dataplex/dap-cert-generator
      
   ```bash
   cd ~/conjur_installer
   wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task05/dap-certificate.tgz
   sudo docker cp ./dap-certificate.tgz conjur-appliance:/tmp/dap-certificate.tgz
   ```

3.	Let's configure the DAP master instance and import the cert
   ```bash
   sudo docker exec -it conjur-appliance bash
   evoke configure master --accept-eula -h master-dap.cyberarkdemo.com --master-altnames "master-dap.cyberarkdemo.com" -p <your design password> cyberark
   *** Remark: Your Conjur Master is Internet faceing, please use a complex password for the Conjur Master: <your design password>
   cd /tmp
   tar -zxvf dap-certificate.tgz
   evoke ca import --root /tmp/dc1-ca.cer.pem
   evoke ca import --key follower-dap.key.pem follower-dap.cer.pem
   evoke ca import --key master-dap.key.pem --set master-dap.cer.pem
   ```
   *** Remark: Make sure all step output without any error

4. Clean up the cert file and exit back to Jump Host Shell
   ```bash
   rm dap-certificate.tgz *.pem
   exit
   ```

5. Setup conjur CLI and load initial policy
- Create conjur cli alias
   ```bash
   alias conjur='docker run --rm -it --network host -v $HOME:/root -it cyberark/conjur-cli:5'
   ```
      -Remark: You may add the alias to your .bash_profile, so you don't have to run alias when you login next time
- Init the Conjur Master
   ```bash
   conjur init -u https://master-dap.cyberarkdemo.com   
   ```
   Key|Value
   ---|-----
   Trust this certificate|yes
   acccount name|cyberark
   
6. Check your Conjur Master is running good
   ```
   6.1. Add a Inbound Rules in your AWS EC2 Jump Host Security Group to allow external access to your Jump Host TCP Port 443
   6.2. Access your Conjur Web UI (that will be your Jump Host AWS public dns name)
        Example: https://ec2-xxxxxxxxxx.ap-east-1.compute.amazonaws.com/
   6.3. Login as admin and use your Conjur Master password in step 3
        If your Conjur Master is running good, you should be seeing samilar UI in below
   ```
   
   ### Login UI
   ![Architecture](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/04-ConjurLoginUI.JPG)
   
   ### Conjur Master UI
   ![Architecture](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/04-ConjurLandingPage.JPG)

7. Load initial root policy to Conjur Master

   ```
   cd ~
   mkdir -p ~/conjur_policy
   wget https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/raw/main/Task05/root.yaml
   conjur authn login -u admin
   conjur policy load root /root/conjur_policy/root.yaml
   ```
   ```
   You should see this output
   Loaded policy 'root'
   {
       "created_roles": {
       },
       "version": 1
    }
    ```
