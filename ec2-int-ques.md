# ✅ EC2 Interview Questions and Answers

A curated list of common EC2-related interview questions and answers for DevOps and Cloud Engineering roles.

---

### ✅ **Q1: What are some of the key EC2 instance types you’ve worked with, and how do you choose the right one?**

**Answer:**  
I’ve worked with several EC2 instance types:
- **t3/t4g**: Cost-efficient general-purpose for small workloads.
- **m5/m6i**: Balanced compute/memory for app servers and Jenkins agents.
- **c5/c6g**: Compute-optimized for CI workloads or build servers.
- **r5/r6g**: Memory-optimized for caching layers and in-memory databases.
- **p3/g4**: For ML workloads and GPU-intensive tasks.

**Selection factors:**
- **Workload type**: Compute vs memory vs storage intensive.
- **Burstable vs consistent performance**
- **Cost optimization** with Spot Instances or Graviton (ARM-based) where possible.
- **Compatibility** with existing software stacks.

---

### ✅ **Q2: How do you automate EC2 instance provisioning using Terraform or CloudFormation?**

**Answer:**  
In **Terraform**, I use the `aws_instance` or `aws_launch_template` resource:
- Define AMI, instance type, key pair, tags, security groups, and user data.
- Attach IAM roles, EBS volumes, and monitoring settings.
- Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t3.micro"
  key_name      = "team-key"
  user_data     = file("init.sh")
  tags = {
    Name = "web-server"
  }
}

