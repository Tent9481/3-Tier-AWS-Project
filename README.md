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
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 10 15 30 AM" src="https://github.com/user-attachments/assets/e8a25014-4a80-405d-98bf-b4b95646231e" />
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 10 15 30 AM" src="https://github.com/user-attachments/assets/e8a25014-4a80-405d-98bf-b4b95646231e" />
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 10 15 53 AM" src="https://github.com/user-attachments/assets/09109c0a-bd12-4b28-a9cc-cb3adc6e1d61" />
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 10 16 14 AM" src="https://github.com/user-attachments/assets/34a61db6-6356-4053-9405-86f759312a85" />
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 10 16 34 AM" src="https://github.com/user-attachments/assets/7421c120-fed6-4343-9f1e-175a69fc549f" />
<img width="1000" height="500" alt="Screenshot 2026-06-24 at 10 16 49 AM" src="https://github.com/user-attachments/assets/0c4fcfeb-a2b4-4668-99c0-5ee74e5a561e" />

___

### Step 8: Create Test Web Server, Install Packages (Nginx, Node.js (React)), Test Connections
- [Test Web-Server Commands](https://github.com/Tent9481/3-Tier-AWS-Project/blob/main/AWS_Project1-main/web-server-commands)
- Create AMI.
- Create launch template using AMI.
- Create target group.
- Create external load balancer.
- Create autoscaling group.

Repeat the same steps for the Web server.

___

### Step 9: Configure a Route 53 DNS Record for the External Application Load Balancer

> **Note:** This step is optional and requires a registered domain name. Since I did not own a domain during this project, I was unable to complete this configuration. If you have a domain name or plan to purchase one, follow the steps below to map your domain to the external Application Load Balancer (ALB).

- Create a **Hosted Zone** in Amazon Route 53 for your domain.

  <img width="1000" height="500" alt="Hosted Zone Creation" src="https://github.com/user-attachments/assets/0493fcc5-7b5b-4ac5-9ca0-4cfb4b6b538e" />

- Create a **CNAME** (or **Alias A**) record that points your domain or subdomain to the DNS name of the external Application Load Balancer.

  <img width="1000" height="500" alt="DNS Record Creation" src="https://github.com/user-attachments/assets/a46e7ca3-fb6e-4735-b6ef-7e9cdc83afa9" />

___

### Step 10: Configure CloudWatch Alarms and SNS Notifications

To monitor the health and performance of the application, I created a CloudWatch alarm based on the **CPU Utilization** metric of the **Application Tier Auto Scaling Group (ASG)**. Whenever the CPU utilization exceeds the defined threshold, CloudWatch triggers an Amazon SNS notification, allowing administrators to take immediate action.

The screenshots below demonstrate the process of creating the CloudWatch alarm and associating it with an SNS topic for email notifications.

<img width="1000" height="500" alt="CloudWatch Alarm Configuration" src="https://github.com/user-attachments/assets/3f2ce435-7173-4e8a-85dd-ec2357a971b0" />

<img width="1000" height="500" alt="Select Metric" src="https://github.com/user-attachments/assets/0c8445f8-4acf-42c3-af6a-e1e7f17b06db" />

<img width="1000" height="500" alt="Configure Threshold" src="https://github.com/user-attachments/assets/5f92a9a2-36bf-4a6e-8cd7-2d3eef68c851" />

<img width="1000" height="500" alt="Configure SNS Notification" src="https://github.com/user-attachments/assets/7ff0d24a-42d7-4a14-887d-31b7159f3c22" />

<img width="1000" height="500" alt="Review Alarm" src="https://github.com/user-attachments/assets/29089040-adfc-40b8-93fc-760e78701426" />


> **Note:** In this project, the alarm was configured for the Application Tier ASG. The same approach can be applied to the Web Tier ASG or any other AWS resource that requires monitoring and alerting.

___

### Step 11: Configure AWS CloudTrail

AWS CloudTrail was enabled to record and monitor all API activities performed within the AWS account. CloudTrail provides an audit trail of user actions and service events, helping with security monitoring, troubleshooting, governance, and compliance.

Follow the steps below to create a CloudTrail trail and start capturing account activity:

<img width="1000" height="500" alt="Create CloudTrail Trail" src="https://github.com/user-attachments/assets/288d7e05-c7b0-4627-997a-8ccc38f877a6" />

<img width="1000" height="500" alt="CloudTrail Configuration" src="https://github.com/user-attachments/assets/01e12b5a-da9b-4e5e-ab65-d91af5247ebe" />

> **Note:** CloudTrail records management events such as resource creation, modification, and deletion across AWS services. These logs can be stored in Amazon S3 for auditing and compliance purposes.

___

## Can Access the application using External Load Balancer
<img width="1511" height="901" alt="Screenshot 2026-06-24 at 10 52 34 AM" src="https://github.com/user-attachments/assets/f3bc5475-3a1d-4e89-868b-105cbc529027" />

___

## Conclusion

In this project, I successfully designed and implemented a production-ready 3-tier architecture on AWS. The solution consists of a Web Tier, Application Tier, and Database Tier deployed in a VPC, following security and high-availability best practices.

Key AWS services such as EC2, Application Load Balancer, Auto Scaling Groups, RDS, S3, IAM, CloudWatch, SNS, and CloudTrail were integrated to create a scalable, secure, and highly available environment. The architecture ensures controlled access between tiers, supports automatic scaling based on demand, and provides comprehensive monitoring, alerting, and auditing capabilities.


This project demonstrates the core principles of cloud architecture, including scalability, security, reliability, fault tolerance, and operational excellence, making it a strong foundation for deploying real-world production workloads on AWS.













