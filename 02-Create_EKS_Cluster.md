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
[default]
aws_access_key_id=<IAM User access key where you created in step 1.0>
aws_secret_access_key=<IAM User secret access key where you created in step 1.0>



1.0. Update you Linux JumpHost Software Package
```bash
sudo yum update
```
