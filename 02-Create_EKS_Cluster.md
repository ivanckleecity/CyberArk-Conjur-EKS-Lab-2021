# Objectives
- add something

### 1.0. Create IAM User for manage EKS Cluster
- Create a IAM user with necessary permission for it to create and manage EKS cluster
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
-{
-    "Account": "409556437035",
-    "UserId": "AIDAV6W3XJAVSP2IUKHIP",
-    "Arn": "arn:aws:iam::409556437035:user/ivanlee-admin"
-}

1.0. Update you Linux JumpHost Software Package
```bash
sudo yum update
```
