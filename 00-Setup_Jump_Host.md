# Objectives
- Setup a Jump Host to access the EKS Envirunment
- For testing purposes, we will reduce the number or EC2 Instances, So we will setup our CyberArk DAP Master and Testing MySQL DataBase Server in the same Jump Host

#Install Jump Host.
1. Login to your AWS Console
2. Create VPC (Give a VPC name tag and assign IPv4 CIDR block, then all others can be default setting)
   ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-vpc-setup01.PNG)
