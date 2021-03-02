# Objectives
Deploy the Follower to your EKS Cluster Node

### 1.0. Collect those yaml files
authn-k8s-cluster.yaml

### 2.0. Create Follower namespace and service account
1. Log in to the Jump Host
   ```bash
   kubectl create namespace dap
   ```
2. Create service account for conjur follower deployment 
   ```bash
   kubectl create serviceaccount conjur-cluster -n dap
   ```
