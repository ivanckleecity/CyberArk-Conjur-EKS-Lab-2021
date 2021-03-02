# CyberArk DAP AWS EKS Integration Lab 2021
This is a tutorial on how to secure secrets of AWS EKS applications by CyberArk Secrets Manager Conjur.   
We will cover deploying Conjur Master, Conjur follower instances by follower seed fetcher.
Conjur Secretless Broker & inital container will also be covered in this tutorial.

## Overview

EKS is used as platform to host the [demo app](https://github.com/jeepapichet/cityapp)
The application will connect to a MySQL database to retreive data, and during authenication, [secrets](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Get%20Started/key_concepts/secrets.html) will be used by the application.

[CyberArk Conjur](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Get%20Started/WhatIsConjur.html) is used in this tutorial to secure & manage the secrets.   


## Architecture

![Architecture](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/architecture_eks.JPG)

## Technical Procedure

### Prerequisite
- You passed CyberArk CPE or CDE
- You have basic understanding Linux installation and administration
- You have basic understanding AWS configuration and administration
- You need to know how to config AWS Route 53 to setup Internal DNS for your lab envirunment
- You have basic understanding EKS Kubernetes setup and administration

### [Task 0: Setup Jump Host](00-Setup_Jump_Host.md)

### [Task 1: Install Necessary Software](01-Install_Necessary_Software.md)

### [Task 2: Create EKS Cluster](02-Create_EKS_Cluster.md)

### [Task 3: Setup DataBase Server](03-Setup_DataBase_Server.md)

### [Task 4: Deploy App with Embedded Secret](04-Deploy_App_with_Embedded_Secret.md)

### [Task 5: Setup Conjur Master Server](05-Setup_Conjur_Master.md)

### [Task 6: Deploy Follower with Seed Fetcher](06-Deploy_Follower_with_Seed_Fetcher.md)
