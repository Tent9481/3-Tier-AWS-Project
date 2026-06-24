# 3-Tier-AWS-Project
## This project demonstrates the implementation of a production-ready 3-tier architecture on AWS. The infrastructure is designed for scalability, high availability, security, monitoring, and fault tolerance. AWS services used in this project include VPC, EC2, S3, Application Load Balancer, Auto Scaling Group, IAM, CloudWatch, SNS, CloudTrail, and RDS.
<img width="1214" height="548" alt="3TierArch 0486e7150e53d305d1c2" src="https://github.com/user-attachments/assets/6eb36c04-037f-4359-bacb-dd35560e20ae" />

### Step 1: Download Code from GitHub in Your Local System
___

### Step 2: Create Two S3 Buckets
- Create one S3 bucket for storing web-server & app-server code.
- Upload the code to your S3 from your local system.
- Create another S3 bucket for VPC flow logs.
<img width="1507" height="802" alt="Screenshot 2026-06-24 at 9 21 47 AM" src="https://github.com/user-attachments/assets/1b194477-495b-46ef-a8e1-4640602abfea" />
<img width="1510" height="798" alt="Screenshot 2026-06-24 at 9 22 10 AM" src="https://github.com/user-attachments/assets/94d8511b-d43d-4c1f-8dd4-a51f5743b701" />
In my case "3tier-22-06-2026" bucket-name to store the code and "vpc-flowlogs-22-06-2026" bucket-name for VPC flow logs.
---
