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

---

### Step 5: Create Security Groups
1. External-Load-Balancer-SG --> HTTP (80): 0.0.0.0/0.
2. Web-Tier-SG --> HTTP --> Ext-LB-SG.
3. Internal-Load-Balancer-SG --> HTTP --> Web-Tier-SG.
4. App-Tier-SG --> Port 4000 --> Internal-LB-SG.
5. DB-Tier-SG --> MySQL (3306) --> App-Tier-SG.

<img width="1510" height="804" alt="Screenshot 2026-06-24 at 10 03 11 AM" src="https://github.com/user-attachments/assets/7e5f7ab7-c2bd-4ce3-9306-2fb8af222753" />

___

### Step 6: Create DB Subnet Group & RDS
- Create DB subnet group.
- Create RDS - Multi-AZ.
- Place them in DB subnet group created above.

<img width="1505" height="670" alt="Screenshot 2026-06-24 at 10 06 05 AM" src="https://github.com/user-attachments/assets/57a2a883-1594-497b-b1ce-4bb594d41c5b" />
<img width="1505" height="670" alt="Screenshot 2026-06-24 at 10 06 05 AM" src="https://github.com/user-attachments/assets/22f995f6-bddc-4936-badd-001fd948ff3f" />

> **Note:** Amazon RDS Multi-AZ deployments are not eligible under the AWS Free Tier. Additionally, when using the Free Tier, RDS instances can only be deployed in the Default VPC. Deploying an RDS instance in a custom VPC may incur charges.

___

### Step 7: Create Test App Server, Install Packages, Test Connections
- [Test App-Server Commands](https://github.com/Tent9481/3-Tier-AWS-Project/blob/main/AWS_Project1-main/app-server-commands)
- Create AMI.
- Create launch template using AMI.
- Create target group.
- Create internal load balancer.
- Create autoscaling group.
- Edit nginx.conf file in local system by adding Internal-LB-DNS & upload the file in S3.
<img width="1508" height="802" alt="Screenshot 2026-06-24 at 10 15 53 AM" src="https://github.com/user-attachments/assets/09109c0a-bd12-4b28-a9cc-cb3adc6e1d61" />
<img width="1508" height="840" alt="Screenshot 2026-06-24 at 10 15 30 AM" src="https://github.com/user-attachments/assets/e8a25014-4a80-405d-98bf-b4b95646231e" />
<img width="1509" height="803" alt="Screenshot 2026-06-24 at 10 16 14 AM" src="https://github.com/user-attachments/assets/34a61db6-6356-4053-9405-86f759312a85" />
<img width="1512" height="836" alt="Screenshot 2026-06-24 at 10 16 34 AM" src="https://github.com/user-attachments/assets/7421c120-fed6-4343-9f1e-175a69fc549f" />
<img width="1512" height="840" alt="Screenshot 2026-06-24 at 10 16 49 AM" src="https://github.com/user-attachments/assets/0c4fcfeb-a2b4-4668-99c0-5ee74e5a561e" />

___







