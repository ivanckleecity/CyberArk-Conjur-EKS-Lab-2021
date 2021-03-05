# CyberArk DAP AWS EKS Integration Lab 2021
This is a tutorial share to you on how to secure secrets of AWS EKS applications by CyberArk Secrets Manager Conjur.   
We will cover deploying Conjur Master, Conjur follower instances by follower seed fetcher.
Conjur Secretless Broker & inital container will also be covered in this tutorial.

[CyberArk Conjur](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Get%20Started/WhatIsConjur.html) is used in this tutorial to secure & manage the secrets.   

## Prerequisite
- You passed CyberArk CPE or CDE
- You completed CyberArk Conjur Fundamentals Course
- You have basic understanding Linux installation and administration
- You have basic understanding AWS configuration and administration
- You need to know how to config AWS Route 53 to setup Internal DNS for your lab envirunment
- You have basic understanding EKS Kubernetes setup and administration

## Architecture
- EKS is used as platform to host the [demo app](https://github.com/jeepapichet/cityapp). The application will connect to a MySQL database to retreive data, and during authenication, [secrets](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Get%20Started/key_concepts/secrets.html) will be used by the applicati

![Architecture](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/architecture_eks.JPG)

## Lab Guide
- Task 0 to 1 is to create AWS envirunment to run the Jump Host and Setup the Jump Host
- Task 2 to 4 is to create an EKS envirunment to run a contrainer App to access a External Database. The External Database will be seating in your Jump Host
- Task 5 to 7 is to setup CyberArk Conjur Secrets Manager to protect containerized applications and DevOps tools secure access to resources. The lab will show case how to setup and use CyberArk Summon and Secretless Broker 


### [Task 0: Setup Jump Host](00-Setup_Jump_Host.md)

### [Task 1: Install Necessary Software in Jump Host](01-Install_Necessary_Software.md)

### [Task 2: Create EKS Cluster](02-Create_EKS_Cluster.md)

### [Task 3: Setup DataBase Server](03-Setup_DataBase_Server.md)

### [Task 4: Deploy App with Embedded Secret](04-Deploy_App_with_Embedded_Secret.md)

### [Task 5: Setup Conjur Master Server](05-Setup_Conjur_Master.md)

### [Task 6: Deploy Follower with Seed Fetcher](06-Deploy_Follower_with_Seed_Fetcher.md)

### [Task 7: Deploy App to EKS Cluster with CyberArk Summon Secrets Injection](07-Deploy_App_with_Summon_Secrets_Injects.md)

### [Task 8: Deploy App to EKS Cluster with CyberArk Secretless Broker](08-Deploy_App_with_Cyberark_Secretless_Broker.md)
