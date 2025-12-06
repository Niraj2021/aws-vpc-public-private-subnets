# AWS VPC with Public & Private Subnets
This project explains how to create a safe and production-ready network on AWS.
It uses public subnets, private subnets, a Load Balancer, and NAT Gateways.

📘 What You Are Building
You will build a network (called a VPC) that looks like this:
The public subnets have:
A Load Balancer (receives traffic from users)
A NAT Gateway (lets private servers reach the internet safely)
The private subnets have:
EC2 servers (your actual application runs here)
These servers cannot be accessed from the internet
But they can reach the internet through the NAT Gateway

Daigram

<img width="549" height="422" alt="image" src="https://github.com/user-attachments/assets/afe32e91-2510-44c3-b7a1-5276c9f863a3" />

⭐ Why This Setup?

This setup is used in real production systems because:

Servers are safe (not exposed to internet)

Traffic is balanced across multiple servers

System stays online even if one Availability Zone fails

Servers can still get updates from the internet using NAT Gateway

🛠️ Step-by-Step — How to Create This Architecture

Below are simple steps anyone can follow in AWS.

1️⃣ Create a VPC

Open AWS Console

Go to VPC → “Create VPC”

Choose CIDR like:

10.0.0.0/16

2️⃣ Create 4 Subnets

You need:

Subnet Type	AZ	Example Name
Public	A	public-subnet-a
Public	B	public-subnet-b
Private	A	private-subnet-a
Private	B	private-subnet-b

Make sure to:

Select one subnet per Availability Zone (A and B)

Mark public subnets with "Auto-assign public IP: ON"

3️⃣ Create and Attach an Internet Gateway

Go to Internet Gateway

Click Create

Attach it to your VPC

Update the public subnet route table:

Add route:

0.0.0.0/0 → Internet Gateway

4️⃣ Create NAT Gateways (One per Public Subnet)

Go to NAT Gateways

Choose each public subnet

Allocate an Elastic IP

Create 2 NAT Gateways (one per AZ)

Update private subnet route tables:

0.0.0.0/0 → NAT Gateway


This lets private servers reach the internet safely.

5️⃣ Create an Application Load Balancer

Go to EC2 → Load Balancers

Create ALB

Select both public subnets

Create a Target Group

You will attach EC2 instances here later

6️⃣ Create an Auto Scaling Group

Create a Launch Template for EC2

Choose AMI, instance type, etc.

Create Auto Scaling Group

Choose private subnets only

Attach it to your Target Group (ALB)

Now:

Users → ALB → Private EC2 servers

7️⃣ (Optional but Recommended) Add S3 Gateway Endpoint

This lets EC2 connect to S3 without using NAT Gateway.
