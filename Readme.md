# EKS Cluster Terraform Project

This Terraform project creates an Amazon EKS (Elastic Kubernetes Service) cluster with the following specifications:
- Cluster name: `devops-assignment-cluster`
- Region: `ap-south-1`
- Kubernetes version: `1.30`
- Node group: 2 `t2.micro` instances (AWS free tier compatible)

## Infrastructure Components

This Terraform configuration creates:

- VPC with public and private subnets across 3 availability zones
- NAT Gateway for private subnet connectivity
- IAM roles and policies for EKS cluster and node groups
- EKS cluster with version 1.30
- EKS node group with 2 t2.micro instances

## Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) (v1.0.0 or newer)
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- [kubectl](https://kubernetes.io/docs/tasks/tools/) for interacting with the cluster after creation

## Usage

### 1. Clone the repository

```bash
git clone <repository-url>
cd eks-terraform-project
```

### 2. Configure AWS credentials

Set your AWS credentials using environment variables (recommended):

```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_DEFAULT_REGION="ap-south-1"
```
or, you can pass the AWS credentials as variables to Terraform:
terraform apply -var="aws_access_key=xxx" -var="aws_secret_key=yyy"
### 3. Initialize Terraform

```bash
terraform init
```

### 4. Review the execution plan

```bash
terraform plan
```

### 5. Apply the configuration

```bash
terraform apply
```

When prompted, type `yes` to confirm.

The creation process takes approximately 15-20 minutes.

### 6. Configure kubectl

After the EKS cluster is created, configure kubectl to interact with it:

```bash
aws eks update-kubeconfig --name devops-assignment-cluster --region ap-south-1
```

### 7. Verify the cluster

```bash
kubectl get nodes
```

## Access Control

The EKS cluster is configured with:
- Private endpoint access (for resources within VPC)
- Public endpoint access (for administration)

## Outputs

The following outputs are generated but marked as sensitive:
- `cluster_endpoint`: The endpoint URL for the EKS cluster API server
- `cluster_security_group_id`: The security group ID attached to the EKS cluster

To retrieve sensitive outputs:
```bash
terraform output cluster_endpoint
terraform output cluster_security_group_id
```

## Clean Up

To destroy the resources when you're done:

```bash
terraform destroy
```

When prompted, type `yes` to confirm.

## Security Considerations

- IAM roles follow the principle of least privilege
- Private subnets are used for EKS nodes for enhanced security
- Security group access is restricted to necessary protocols and ports

## Customization

To modify the deployment:
- Adjust the VPC CIDR and subnet ranges in the VPC module
- Change the instance types or scaling configuration in the node group
- Modify the Kubernetes version by changing the version parameter


