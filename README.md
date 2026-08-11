# 🚀 EKS Cluster Deployment

![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge\&logo=terraform\&logoColor=white)
![AWS EKS](https://img.shields.io/badge/Amazon%20EKS-FF9900?style=for-the-badge\&logo=amazoneks\&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge\&logo=amazonaws\&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge\&logo=jenkins\&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge\&logo=kubernetes\&logoColor=white)

> **Automated Amazon EKS cluster deployment using Terraform and Jenkins CI/CD.**

---

## 📌 About The Project

This project demonstrates how to provision an **Amazon Elastic Kubernetes Service (EKS)** cluster using **Terraform** and automate infrastructure deployment using **Jenkins**.

The Terraform configuration dynamically discovers the **default AWS VPC and its subnets**, creates an EKS cluster, configures the required IAM roles and policies, and provisions an **EKS managed node group**.

The Jenkins pipeline provides a simple way to manage the infrastructure by allowing the user to select either **Apply** or **Destroy**.

### ✨ Key Features

* ☁️ Amazon EKS cluster provisioning
* 🏗️ Infrastructure as Code using Terraform
* 🔍 Dynamic discovery of the AWS default VPC
* 🌐 Dynamic discovery of default VPC subnets
* ☸️ Amazon EKS Kubernetes cluster
* 👨‍💻 EKS managed node group
* 🔐 IAM roles and AWS managed policies
* ⚙️ Jenkins CI/CD automation
* 🔄 Terraform Apply and Destroy automation
* 📦 ON_DEMAND EC2 worker nodes
* 🏷️ Development environment node labeling
* 🛠️ AWS CLI and kubectl integration

---

## 🏗️ Architecture

```text
                         ┌──────────────────────┐
                         │        GitHub        │
                         │ Terraform + Jenkins  │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       Jenkins        │
                         │    CI/CD Pipeline    │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │      Terraform       │
                         │ Init → Plan → Apply  │
                         └──────────┬───────────┘
                                    │
                                    ▼
                 ┌──────────────────────────────────┐
                 │            AWS Cloud              │
                 │                                  │
                 │          Default VPC             │
                 │                │                 │
                 │                ▼                 │
                 │       ┌─────────────────┐        │
                 │       │   Amazon EKS     │        │
                 │       │  Kubernetes 1.35 │        │
                 │       └────────┬────────┘        │
                 │                │                 │
                 │                ▼                 │
                 │       ┌─────────────────┐        │
                 │       │ Managed Node     │        │
                 │       │ Group            │        │
                 │       │ my-node-group    │        │
                 │       └────────┬────────┘        │
                 │                │                 │
                 │                ▼                 │
                 │       ┌─────────────────┐        │
                 │       │ EC2 Worker Node │        │
                 │       │    t3.small     │        │
                 │       └─────────────────┘        │
                 └──────────────────────────────────┘
```

---

## 📂 Project Structure

```text
eks-cluster-deployment/
│
├── main.tf
├── provider.tf
├── iam.tf
├── jenkinsfile
└── README.md
```

### 📄 File Description

| File          | Description                                                |
| ------------- | ---------------------------------------------------------- |
| `main.tf`     | Creates the EKS cluster and managed node group             |
| `provider.tf` | Configures the AWS provider and region                     |
| `iam.tf`      | Creates EKS cluster and worker-node IAM roles and policies |
| `jenkinsfile` | Jenkins pipeline for Terraform automation                  |
| `README.md`   | Project documentation                                      |

---

## 🛠️ Technologies Used

| Technology    | Purpose                          |
| ------------- | -------------------------------- |
| **AWS EKS**   | Managed Kubernetes control plane |
| **AWS EC2**   | EKS worker nodes                 |
| **AWS IAM**   | IAM roles and permissions        |
| **AWS VPC**   | Networking for EKS               |
| **Terraform** | Infrastructure as Code           |
| **Jenkins**   | CI/CD automation                 |
| **GitHub**    | Source code management           |
| **kubectl**   | Kubernetes cluster management    |
| **AWS CLI**   | AWS and EKS management           |

---

## ☁️ AWS Configuration

### AWS Region

```text
ap-south-1
```

The project uses the AWS **Mumbai region**.

### VPC Configuration

The project uses the **AWS Default VPC**.

Terraform dynamically retrieves the default VPC:

```hcl
data "aws_vpc" "default" {
  default = true
}
```

The subnets associated with the default VPC are retrieved automatically:

```hcl
data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}
```

This avoids hard-coding VPC and subnet IDs.

---

## ☸️ EKS Configuration

| Configuration      | Value               |
| ------------------ | ------------------- |
| Cluster Name       | `eks-cluster`       |
| Kubernetes Version | `1.35`              |
| AWS Region         | `ap-south-1`        |
| VPC                | Default VPC         |
| Subnets            | Default VPC Subnets |
| Node Group         | `my-node-group`     |
| Instance Type      | `t3.small`          |
| Capacity Type      | `ON_DEMAND`         |
| Disk Size          | `20 GB`             |
| Desired Nodes      | `1`                 |
| Minimum Nodes      | `1`                 |
| Maximum Nodes      | `2`                 |
| Max Unavailable    | `1`                 |
| Node Label         | `env=dev`           |

---

## 🔐 IAM Configuration

### EKS Cluster IAM Role

Role:

```text
eks-cluster-example-2
```

Policies:

```text
AmazonEKSClusterPolicy
AmazonEKSVPCResourceController
```

The role is assumed by:

```text
eks.amazonaws.com
```

### EKS Worker Node IAM Role

Role:

```text
eks-node-role-2
```

Policies:

```text
AmazonEKSWorkerNodePolicy
AmazonEKS_CNI_Policy
AmazonEC2ContainerRegistryReadOnly
```

The role is assumed by:

```text
ec2.amazonaws.com
```

---

## ⚙️ Prerequisites

Install the following tools:

* AWS CLI
* Terraform
* kubectl
* Git
* Jenkins
* AWS Account with appropriate permissions

### Verify Installation

```bash
terraform version
```

```bash
aws --version
```

```bash
kubectl version --client
```

```bash
git --version
```

---

## 🔑 AWS Configuration

Configure AWS credentials:

```bash
aws configure
```

Enter:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name: ap-south-1
Default output format
```

Verify the AWS identity:

```bash
aws sts get-caller-identity
```

> ⚠️ **Security:** Never commit AWS access keys, secret keys, passwords, or other sensitive credentials to GitHub.

---

## 🚀 Terraform Deployment

## 1. Clone the Repository

```bash
git clone https://github.com/shubham-rasal-123/eks-cluster-deployment.git
```

```bash
cd eks-cluster-deployment
```

---

## 2. Initialize Terraform

```bash
terraform init
```

---

## 3. Format Terraform Files

```bash
terraform fmt
```

---

## 4. Validate Terraform Configuration

```bash
terraform validate
```

Expected result:

```text
Success! The configuration is valid.
```

---

## 5. Create Terraform Plan

```bash
terraform plan
```

Review the resources Terraform plans to create.

---

## 6. Apply Terraform Configuration

```bash
terraform apply
```

Enter:

```text
yes
```

when Terraform asks for confirmation.

Terraform will create:

```text
Default VPC
     │
     ▼
EKS Cluster
     │
     ▼
Managed Node Group
     │
     ▼
EC2 Worker Nodes
```

---

## ☸️ Connect to the EKS Cluster

After the EKS cluster is created, configure `kubectl`:

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name eks-cluster
```

## Verify Cluster

```bash
kubectl cluster-info
```

### Check Nodes

```bash
kubectl get nodes
```

Example:

```text
NAME                                          STATUS   ROLES    AGE
ip-10-0-x-x.ap-south-1.compute.internal      Ready    <none>   5m
```

---

## 🔄 Jenkins CI/CD Automation

The project contains a Jenkins pipeline:

```text
jenkinsfile
```

The pipeline automates Terraform operations.

## Jenkins Pipeline Flow

```text
GitHub
   │
   ▼
Jenkins
   │
   ▼
Checkout
   │
   ▼
terraform init -reconfigure
   │
   ▼
terraform plan
   │
   ▼
Select ACTION
   │
   ├───────────────┐
   │               │
   ▼               ▼
 apply           destroy
   │               │
   ▼               ▼
Create EKS      Delete EKS
```

---

## 🧩 Jenkins Pipeline Stages

| Stage              | Description                               |
| ------------------ | ----------------------------------------- |
| **Checkout**       | Clones the GitHub repository              |
| **terraform init** | Initializes Terraform with `-reconfigure` |
| **plan**           | Creates the Terraform execution plan      |
| **Action**         | Executes Apply or Destroy                 |

---

## 🎛️ Jenkins ACTION Parameter

The pipeline contains an `ACTION` choice parameter.

Available options:

```text
apply
destroy
```

### 🟢 Apply

Select:

```text
ACTION = apply
```

Jenkins executes:

```bash
terraform apply --auto-approve
```

This provisions the EKS infrastructure.

### 🔴 Destroy

Select:

```text
ACTION = destroy
```

Jenkins executes:

```bash
terraform destroy --auto-approve
```

This removes the Terraform-managed infrastructure.

> ⚠️ **Warning:** The `destroy` option permanently removes the Terraform-managed AWS infrastructure.

---

## 🧹 Destroy Infrastructure

To destroy the infrastructure manually:

```bash
terraform destroy
```

Confirm:

```text
yes
```

Or use Jenkins:

```text
ACTION = destroy
```

---

## 📊 Terraform Workflow

```text
Terraform Configuration
          │
          ▼
   terraform init
          │
          ▼
    terraform fmt
          │
          ▼
  terraform validate
          │
          ▼
    terraform plan
          │
          ▼
   terraform apply
          │
          ▼
      Amazon EKS
          │
          ▼
 Managed Node Group
          │
          ▼
      EC2 Nodes
```

---

## 💡 Terraform Concepts Demonstrated

## Data Sources

### Default VPC

```hcl
data "aws_vpc" "default" {
  default = true
}
```

### Default VPC Subnets

```hcl
data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}
```

---

## EKS Cluster

```hcl
resource "aws_eks_cluster" "this" {
  name     = "eks-cluster"
  role_arn = aws_iam_role.example.arn
  version  = "1.35"
}
```

---

## EKS Managed Node Group

```hcl
resource "aws_eks_node_group" "this" {
  cluster_name    = aws_eks_cluster.this.name
  node_group_name = "my-node-group"
  node_role_arn   = aws_iam_role.worker.arn
}
```

---

## 🎯 Project Objectives

This project demonstrates practical knowledge of:

* ☁️ Amazon EKS
* ☸️ Kubernetes
* 🏗️ Terraform
* 🔐 AWS IAM
* 🌐 AWS VPC
* 👨‍💻 EKS Managed Node Groups
* 🖥️ EC2 Worker Nodes
* 🔧 AWS CLI
* ☸️ kubectl
* ⚙️ Jenkins
* 🔄 CI/CD
* 📦 Infrastructure as Code

---

## 📚 Learning Outcomes

After completing this project, you will understand how to:

1. Configure the AWS provider using Terraform.
2. Retrieve the default VPC using Terraform data sources.
3. Retrieve VPC subnets dynamically.
4. Create an Amazon EKS cluster.
5. Configure IAM roles for EKS.
6. Create an EKS managed node group.
7. Configure EC2 worker nodes.
8. Connect to EKS using `kubectl`.
9. Automate Terraform using Jenkins.
10. Apply and destroy infrastructure through Jenkins.

---

## 🔮 Future Improvements

* [ ] Create a custom VPC
* [ ] Add Terraform variables
* [ ] Add Terraform outputs
* [ ] Add Terraform modules
* [ ] Add remote Terraform backend using S3
* [ ] Add Terraform state locking
* [ ] Add Dev / UAT / Production environments
* [ ] Add Jenkins approval stage
* [ ] Add Kubernetes application deployment
* [ ] Add EKS Add-ons
* [ ] Add CloudWatch monitoring
* [ ] Add logging and monitoring
* [ ] Add public and private subnets
* [ ] Add NAT Gateway
* [ ] Add Application Load Balancer
* [ ] Add autoscaling configuration

---

## ⚠️ Important Notes

## Default VPC -

This project uses the AWS **Default VPC**.

Make sure a default VPC exists in:

```text
ap-south-1
```

### AWS Costs

AWS EKS and EC2 resources may incur charges.

After testing, destroy the infrastructure if it is no longer required:

```bash
terraform destroy
```

### Credentials

Never commit the following to GitHub:

```text
AWS Access Keys
AWS Secret Keys
Passwords
Private Keys
Terraform Secrets
```

---

## 👨‍💻 Author

## Shubham Rasal

-**DevOps Engineer**

`AWS` • `Terraform` • `Kubernetes` • `Jenkins` • `Docker` • `Linux` • `Git`

---

## ⭐ Support

If you found this project useful, consider giving the repository a ⭐ on GitHub.
