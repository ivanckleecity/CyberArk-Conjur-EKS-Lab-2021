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
eksctl create cluster \
> --name <my cluster name> \
> --version 1.18\
> --region <your AWS region> \
> --nodegroup-name <my cluster node name> \
> --node-type t2.medium \
> --nodes 1
```
- Example
```
eksctl create cluster \
> --name test-cluster \
> --version 1.18 \
> --region ap-east-1 \
> --nodegroup-name test-cluster-nodes \
> --node-type t2.medium \
> --nodes 1
```

- If you want to learn more about how to use eksctl to setup and manage AWS EKS Cluster, plesea visit [AWS Online Docs](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

- Creation takes several minutes
- If everything good, you should see several lines of output as the cluster are created.
```
The last line of output is similar to the following example line.
[✓]  EKS cluster "my-cluster" in "us-west-2" region is ready
```
- If you access AWS EKS Web portal, you should the similar UI and showing "Active" and "Status Ready"

![Architecture](https://github.com/ivanckleecity/CyberArk-DAP-EKS-Lap-2021/blob/main/images/EKS_Cluster_Sample_UI.JPG)
