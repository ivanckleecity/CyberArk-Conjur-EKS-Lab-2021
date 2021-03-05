# Objectives
- Setup a Jump Host to access the EKS Envirunment
- For testing purposes, we will reduce the number or EC2 Instances, So we will setup our CyberArk DAP Master and Testing MySQL DataBase Server in the same Jump Host

## Install Jump Host.
1.0. Login to your AWS Console

2.1. Create VPC (If you have your VPC with Internet Gateway and you want to run your Jump Host in your current VPC, please skip step 2.1 to 2.4)
- Give a VPC name tag and assign IPv4 CIDR block, all others setting can be default
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup01.PNG)

2.2. Enable the DNS Hostname 
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup03.PNG)

2.2. Create VPC Subnet 
- Create VPC Subnet
- Select the VPC where you created in step 2.1
- Give a Subnet name tag and assign IPv4 CIDR block, all others setting can be default                                                                             
     ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-vpc-setup02.PNG)

2.3. Create Internet Gateway
- Create Internet Gateway
- Give a name to the Internet Gateway
- Attach to your VPC where you created in step 2.1                                                              
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup06.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup07.PNG)

2.4. Edit Route Tables
- Go to Route Tables
- Select the route table ID where link with your VPC ID (created in step 2.1)
- Select the "Routes tab"
- Click the "Edit routes"
- Add default gateway to the Intenet Gateway where you create in step 2.3                                              
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup08.PNG)

3.0. Setup EC2 Instances

3.1. Launch EC2 Instances
- Start with "Launch instances"
- Select "Amazon Linux 2 AMI"
  ![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-01-Amazon_Linux_2_AMI.PNG)
-

3.2. Configurate the Amazon Linux 2 (Remark: Below is recommended setting for the Linux JumpHost, if you want you can change it)
- Instance Type: t2.medium, 2vCPU, 4G Memory
- Security Group: Create a Security Group for this Linux with SSH Open to you workstation
- Network: Select your VPC and Subent from step 2.1 and 2.2
- Network: Enable assign public IP
- Storage: Assign 20G and gp2
- Create your new SSH key pair for access this Jump Host OR you can reuse your existing SSH key pair
- All others setting, you can use default vaule                                                                        
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup01.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup02.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup03.PNG)

3.3. (Optional) Assign the Elastic IP address to you JumpHost Instance
- Allocate Elastic IP Address to the JumpHost Instance ID where created in the step 3.2
- Select your JumpHost Instance Private IP address                                                                           
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup04.PNG)
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup05.PNG)

3.4. Check if you can access your Linux JumpHost with the ssh key you created in step 3.2
![](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/00-03-EC2_Linux_Setup09.PNG)                                                       
