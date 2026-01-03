# AWS-EKS-Project
## Day 1: VPC and Subnet Setup

### Overview
Set up the foundational AWS networking for the EKS project. This includes a VPC, public and private subnets, a NAT Gateway, and an Internet Gateway. These resources form the base infrastructure for deploying a secure, scalable Kubernetes cluster.

### Resources Created
- **VPC:** Isolated network environment for the EKS infrastructure.
- **Public Subnets (2):** For load balancers and resources that require internet access.
- **Private Subnets (2):** For worker nodes and internal services.
- **Internet Gateway (IGW):** Provides internet access for public subnets.
- **NAT Gateway:** Allows private subnets to access the internet while remaining private.

### Steps
1. Define VPC with CIDR block (e.g., `10.0.0.0/16`) in Terraform.
2. Create **2 public subnets** and **2 private subnets** across availability zones.
3. Attach an **Internet Gateway** to the VPC for public access.
4. Create a **NAT Gateway** in a public subnet for private subnet outbound traffic.
5. Define **route tables**:
   - Public subnets → route to IGW
   - Private subnets → route to NAT Gateway
6. Associate the subnets with respective route tables.
7. Apply Terraform configuration:

terraform init
terraform plan
terraform apply


### EKS-Specific Tags for Subnets
Proper tagging ensures that EKS can automatically recognize and use these subnets.

**Public Subnets:**
- `kubernetes.io/cluster/<cluster-name>` = `owned` → Marks subnet as part of this EKS cluster.
- `kubernetes.io/role/elb` = `1` → Used for internet-facing load balancers.
- Add other Standard tags for tracking:


**Private Subnets:**
- `kubernetes.io/cluster/<cluster-name>` = `owned` → Marks subnet as part of this EKS cluster.
- `kubernetes.io/role/internal-elb` = `1` → Used for internal load balancers and node groups.
- Add Standard tags for tracking:

**Key Notes:**
- `kubernetes.io/cluster/<cluster-name>` is required; without it EKS cannot use the subnet.
- Public subnets host internet-facing load balancers; private subnets host internal services.
- Consistent tagging simplifies automation, cost tracking, and management.
