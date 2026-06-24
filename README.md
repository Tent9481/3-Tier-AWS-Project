# 3-Tier-AWS-Project
## This project demonstrates the implementation of a production-ready 3-tier architecture on AWS. The infrastructure is designed for scalability, high availability, security, monitoring, and fault tolerance. AWS services used in this project include VPC, EC2, S3, Application Load Balancer, Auto Scaling Group, IAM, CloudWatch, SNS, CloudTrail, and RDS.
<img width="1214" height="548" alt="3TierArch 0486e7150e53d305d1c2" src="https://github.com/user-attachments/assets/6eb36c04-037f-4359-bacb-dd35560e20ae" />

### Step 1: Download Code from GitHub in Your Local System

___

### Step 2: Create Two S3 Buckets
- Create one S3 bucket for storing web-server & app-server code.
- Upload the code to your S3 from your local system.
- Create another S3 bucket for VPC flow logs.
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 9 21 47 AM" src="https://github.com/user-attachments/assets/1b194477-495b-46ef-a8e1-4640602abfea" />
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 9 22 10 AM" src="https://github.com/user-attachments/assets/94d8511b-d43d-4c1f-8dd4-a51f5743b701" />
In my case "3tier-22-06-2026" bucket-name to store the code and "vpc-flowlogs-22-06-2026" bucket-name for VPC flow logs.

---

### Step 3: Create IAM Role with Policies
- **Amazon S3 Read-Only Access:** Granted EC2 instances read-only permissions to securely retrieve application code and artifacts from Amazon S3.
- **Amazon SSM Managed Instance Core:** Enabled secure access to EC2 instances through AWS Systems Manager Session Manager, eliminating the need to open SSH (port 22). This approach enhances security by reducing the attack surface and removing the dependency on direct SSH access.
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 9 34 41 AM" src="https://github.com/user-attachments/assets/f8e78a58-2c56-4b50-ac4f-a3e74e32ac8a" />
As you can see, Role has 2 permissions. **AmazonS3ReadOnlyAccess and AmazonSSMManagedInstanceCore**.

___

### Step 4: Create VPC, Subnets, IGW, NAT-GW, RT
- Enable auto-assign public IP for web-tier public subnets.
- Create flow logs for VPC & use the S3 bucket created above.
<img width="1512" height="803" alt="Screenshot 2026-06-24 at 9 55 32 AM" src="https://github.com/user-attachments/assets/dc17a0cd-873a-4aab-a38c-69feb16e40da" />
The VPC is designed using a 3-tier architecture consisting of Web, Application, and Database tiers distributed across multiple Availability Zones for high availability.
- **Web Tier:** Deployed in public subnets with a route to the Internet Gateway (IGW), allowing inbound and outbound internet access.
- **Application Tier:** Deployed in private subnets with outbound internet connectivity provided through a NAT Gateway, preventing direct access from the internet.
- **Database Tier:** Deployed in private subnets and routes outbound traffic through the NAT Gateway while remaining isolated from direct internet access, enhancing security.


