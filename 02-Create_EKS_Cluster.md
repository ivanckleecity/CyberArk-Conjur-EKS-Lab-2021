# Objectives
- add something

### 1.0. Create IAM User for manage EKS Cluster
- Create a IAM user with necessary permission for it to create and manage EKS cluster
- That IAM user need Programmatic access
- Create access key for this user and Please keep the Access Key safely

### 1.1. Create AWS Credential file in the JumpHost
```bash
vi the ~/.aws/credential
```
- Add the following content into the credential file
```bash
[default]
aws_access_key_id=<IAM User access key where you created in step 1.0>
aws_secret_access_key=<IAM User secret access key where you created in step 1.0>
```
- Example
```bash
[default]
aws_access_key_id=1234567890123
aws_secret_access_key=tladkfnlsdwmefhskjdfjaerj21
```


1.0. Update you Linux JumpHost Software Package
```bash
sudo yum update
```
