# Objectives
Install all necessary software in your jump host for your next labs, those software are 
git
jq
binutils
gcc
homebrew
eksctl
aws-iam-authenticator
docker

1.0. Update your Linux JumpHost Software Package
- Login to your jump host
```bash
sudo yum update
```

2.0 Install git, jq, binutils and gcc
```bash
sudo yum install git -y
sudo yum install jq
sudo yum install binutils
sudo yum install gcc
```
3.0. Install eksctl for creating and managing EKS clusters

3.1. Install Homebrew
```bash
mkdir -p ~/.linuxbrew/bin
git clone https://github.com/Homebrew/brew ~/.linuxbrew/Homebrew
cd  ~/.linuxbrew/bin 
ln -s ../Homebrew/bin/brew ~/.linuxbrew/bin
eval $(~/.linuxbrew/bin/brew shellenv)
brew tap weaveworks/tap
```

3.2. Install eksctl
```bash
brew install weaveworks/tap/eksctl
eksctl version
```
3.3 Append the $HOME/.linuxbrew/bin to your user .bash_profile
- Append $HOME/.linuxbrew/bin to the PATH
```bash
vi ~/.bash_profile
```
- Example:
  - PATH=$PATH:$HOME/.local/bin:$HOME/bin:$HOME/.linuxbrew/bin
- Reload .bash_profile to your current shell
```bash
source ~/.bash_profile
```

3.4 Install aws-iam-authenticator
```
cd /usr/local/bin/
sudo curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.9/2020-11-02/bin/linux/amd64/aws-iam-authenticator
sudo chmod 755 ./aws-iam-authenticator
```

4.0. Install Docker
```
cd ~
sudo yum install -y docker
sudo service docker start
sudo groupadd docker
sudo usermod -a -G docker ec2-user
newgrp docker 
```
Check if your Docker running correctly
```bash
docker run hello-world
```
Check if you can see...
"Hello from Docker! This message shows that your installation appears to be working correctly."
