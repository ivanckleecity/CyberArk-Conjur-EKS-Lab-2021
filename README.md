# CyberArk DAP AWS EKS Integration Lab 2021
This is a tutorial on how to secure secrets of AWS EKS applications by CyberArk Dynamic Access Provider (DAP).   
We will cover deploying DAY Master, DAP follower instances by follower seed fetcher.
Secretless Broker & inital container will also be covered in this tutorial.

Extra tech challenges will be included in each sections for quick learners.

## Overview

[OKD](https://www.okd.io) is used as the OpenShift platform to host the [demo app](https://github.com/jeepapichet/cityapp)
The application will connect to a MySQL database to retreive data, and during authenication, [secrets](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Get%20Started/key_concepts/secrets.html) will be used by the application.

[Dynamic Access Provider (DAP)](https://docs.cyberark.com/Product-Doc/OnlineHelp/AAM-DAP/Latest/en/Content/Get%20Started/WhatIsConjur.html) is used in this tutorial to secure & manage the secrets.   


## Architecture

![Architecture](https://github.com/QuincyChengAtWork/DAP-OpenShift-Lab-2020/raw/master/images/architecture.png)


## Technical Procedure

### Prerequisite
 - Access to Smartfile
 - FTP client
 - 7zip or Winzip installed on your workstation
 - VMware Workstation 12 or greater installed on your workstation
 - CyberArk CorePAS installed on VMWare workstation, `CGD-2020-0101-GA` prefered 
 - Sufficient disk space for additional 2 virtual machines (5.6GB for compressed VM and/or 24GB for extracted VM)

### [Preparation: Environment Setup](00-setup.md)
1. Setup CyberArk CorePAS based on CGD
2. Setup 2 Extra VM (DAP Master & OKD)
3. Onboard MySQL Account to CorePAS
4. Setup DAP Master
5. Configure Vault Synchronizer
