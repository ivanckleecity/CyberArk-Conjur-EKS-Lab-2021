# Objectives
- Setup a Jump Host to access the EKS Envirunment
- For testing purposes, we will reduce the number or EC2 Instances, So we will setup our CyberArk DAP Master and Testing MySQL DataBase Server in the same Jump Host

## Install Jump Host.
1. Login to your AWS Console

2.1. Create VPC (Give a VPC name tag and assign IPv4 CIDR block, all others setting can be default)
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup01.PNG)

2.2. Enable the DNS Hostname 
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup03.PNG)

2.2. Create VPN Subnet 
     ![abc](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup02.PNG)

3. Launch EC2 Instances

3.1 Select "Amazon Linux 2 AMI"
    ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-Amazon_Linux_2_AMI.PNG)
