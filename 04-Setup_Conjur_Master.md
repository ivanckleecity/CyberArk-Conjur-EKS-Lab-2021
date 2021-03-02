# Objectives
Install CyberArk Conjur Master

### 1.0. Collect the Conjur Installer and seed fectcher from CyberArk Sales team
1. conjur-appliance_xx.x.x.tar.gz
2. dap-seedfetcher_x.x.x.tar.gz
3. Upload to your Jump Host
4. Remark: the xx.x.x is the Conjur version
   
### 1.1. Install Conjur Master

1. Login to your Jump Host
2. Load the Conjur and seed fetcher containers to your Docker
   ```bash
   docker load -i conjur-appliance_12.0.0.tar.gz
   docker load -i dap-seedfetcher_0.1.5.tar.gz
   docker tag registry.tld/conjur-appliance:12.0.0 conjur-appliance:12.0.0
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
### 1.2. Create eks cluster
