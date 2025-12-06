# AWS VPC with Public & Private Subnets (Production-Ready Architecture)

This repository explains how to design and deploy a secure, scalable, production-grade network on AWS.
The architecture uses public subnets, private subnets, an Application Load Balancer (ALB), NAT Gateways, and Auto Scaling EC2 instances.

📘 Architecture Overview

You will deploy an AWS VPC that includes:

# Public Subnets

Application Load Balancer (ALB) — receives traffic from users

NAT Gateways — allow private EC2 instances to securely access the internet

# Private Subnets

EC2 application servers (Auto Scaling Group)

Not exposed to the internet

Internet access only through NAT Gateway

Secure connection to S3 through a Gateway Endpoint (optional)


# 🖼️ Diagram
<img width="550" height="420" alt="architecture diagram" src="https://github.com/user-attachments/assets/afe32e91-2510-44c3-b7a1-5276c9f863a3" />

# ⭐ Why This Architecture?

This setup is commonly used in real-world production environments because:

Secure: Private servers are not exposed to the internet

Highly Available: Spreads across multiple Availability Zones

Scalable: Auto Scaling Group adjusts capacity automatically

Cost-Efficient: S3 Gateway Endpoint reduces NAT usage

Stable: ALB ensures smooth traffic distribution

# 🛠️ Step-by-Step Guide to Create This Architecture in AWS

These steps are intentionally simple and easy for beginners.

# 1️⃣ Create a VPC

Open AWS Console

Go to VPC → Create VPC

Select VPC only

Enter:

CIDR block: 10.0.0.0/16


Click Create VPC

# 2️⃣ Create 4 Subnets

You need one public and one private subnet in each Availability Zone.

Subnet Type	AZ	Example Name
Public	A	public-subnet-a
Public	B	public-subnet-b
Private	A	private-subnet-a
Private	B	private-subnet-b

# Important:

Enable Auto-assign public IPv4 = ON only for public subnets

Leave public IP OFF for private subnets

# 3️⃣ Create and Attach an Internet Gateway

Open Internet Gateways

Click Create Internet Gateway

Attach it to your VPC

Update public subnet route table:

0.0.0.0/0 → Internet Gateway

# 4️⃣ Create NAT Gateways (1 per Public Subnet)

Open NAT Gateways

Create NAT Gateway in public-subnet-a

Allocate an Elastic IP

Repeat for public-subnet-b

Update private subnet route tables:

0.0.0.0/0 → NAT Gateway


This allows private EC2 instances to access the internet securely.

# 5️⃣ Create an Application Load Balancer

Go to EC2 → Load Balancers

Create an Application Load Balancer

Scheme: Internet-facing

Select both public subnets

Create a Target Group

ALB will forward traffic to EC2 instances (next step)

# 6️⃣ Create an Auto Scaling Group (EC2)

Create a Launch Template

Choose AMI, instance type, security group, etc.

Create an Auto Scaling Group

Select private subnets only

Attach it to the Target Group from the ALB

Now the traffic flow becomes:

Users → ALB → EC2 in private subnets

# 7️⃣ (Optional but Recommended) Add an S3 Gateway Endpoint

This allows EC2 instances to reach S3 without using the NAT Gateway.

Benefits:

Private connection

Reduced NAT costs

Faster S3 access
