# Objectives
- A

## Install Jump Host.
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

1.0. Update you Linux JumpHost Software Package
''sudo yum update


sudo yum install git -y
sudo yum install jq
git clone https://github.com/Homebrew/brew ~/.linuxbrew/Homebrew
mkdir ~/.linuxbrew/bin
ln -s ../Homebrew/bin/brew ~/.linuxbrew/bin
eval $(~/.linuxbrew/bin/brew shellenv)
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
eksctl version
