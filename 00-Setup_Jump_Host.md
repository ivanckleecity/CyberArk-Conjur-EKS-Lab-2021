# Objectives
- Setup a Jump Host to access the EKS Envirunment
- For testing purposes, we will reduce the number or EC2 Instances, So we will setup our CyberArk DAP Master and Testing MySQL DataBase Server in the same Jump Host

## Install Jump Host.
1.0. Login to your AWS Console

2.1. Create VPC
- Give a VPC name tag and assign IPv4 CIDR block, all others setting can be default
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup01.PNG)

2.2. Enable the DNS Hostname 
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup03.PNG)

2.2. Create VPN Subnet 
- Select the VPC where you created in step 2.1
- Give a Subnet name tag and assign IPv4 CIDR block, all others setting can be default                                                                             
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup02.PNG)

2.3. Create Internet Gateway
- Give a name to the Internet Gateway
- Attach to your VPC where you created in step 2.1
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup06.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup07.PNG)


3.0. Setup EC2 Instances

3.1. Select "Amazon Linux 2 AMI"
    ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-Amazon_Linux_2_AMI.PNG)

3.2. Configurate the Amazon Linux 2 (Remark: Below is recommended setting for the Linux JumpHost, if you want you can change it)
- Instance Type: t2.medium, 2vCPU, 4G Memory
- Security Group: Create a Security Group for this Linux with SSH Open to the World or you workstation
- Network: Select your VPC and Subent from step 2.1 and 2.2
- Network: Enable assign public IP
- Storage: Assign 20G and gp2
- Create your new SSH key pair for access this Jump Host OR you can reuse your existing SSH key pair
- All others setting, you can use default vaule

![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup01.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup02.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup03.PNG)
