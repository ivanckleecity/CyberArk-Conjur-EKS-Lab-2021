# Objectives
Create you own EKS Cluster envirunment for run the lab

### 1.0. Create IAM User for manage EKS Cluster
- We will not mention how to setup and admin AWS IAM user, if you need to learn, please visit [AWS Online Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)
- Suggestion steps
    - Create a IAM user with necessary permission for manage EKS cluster
    - That IAM user need Programmatic access
    - Create access key for this user and Please keep the Access Key safely

### 1.1. Create AWS Credential file in the JumpHost
```bash
aws configure
```
- Following the Answers
```bash
AWS Access Key ID [None]: <Your Key ID>
AWS Secret Access Key [None]: <Your Access Key>
Default region name [None]: <Your AWS Region>
Default output format [None]: <Just Click Enter>

```
- Check your AWS Credential
```bash
aws sts get-caller-identity
```
- Your should see the similar output
```
{
    "Account": "123456789012",
    "UserId": "xyxyxyxyxyxyxx",
    "Arn": "arn:aws:iam::123456789012:user/john"
}
```

### 1.2. Create eks cluster
```bash
eksctl create cluster  --name <your-cluster-name>  --region <your-region>  --nodegroup-name <your-cluster-group-name>  --node-type t2.large --nodes 1
```
- Example
    - eksctl create cluster  --name test-eks-01-ap-east-1  --region ap-east-1  --nodegroup-name test-eks-01-node-ap-east-1  --node-type t2.large --nodes 1

- If you want to learn more about how to use eksctl to setup and manage AWS EKS Cluster, plesea visit [AWS Online Docs](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
