# Objectives
- Please follow those steps to install all necessary software for your next labs

1.0. Update you Linux JumpHost Software Package
```bash
sudo yum update
```

2.0 Install git and jq
```bash
sudo yum install git -y
sudo yum install jq
```
3.0. Install eksctl for creating and managing EKS clusters

3.1. Install Homebrew
```bash
git clone https://github.com/Homebrew/brew ~/.linuxbrew/Homebrew
mkdir ~/.linuxbrew/bin
ln -s ../Homebrew/bin/brew ~/.linuxbrew/bin
eval $(~/.linuxbrew/bin/brew shellenv)
brew tap weaveworks/tap
```
3.2. Install eksctl
```bash
brew install weaveworks/tap/eksctl
eksctl version
```


