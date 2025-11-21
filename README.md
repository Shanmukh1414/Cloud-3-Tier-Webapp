🚀 Three-Tier Web Application on AWS

This repository contains my custom implementation of a production-ready three-tier architecture on AWS, inspired by the AWS Three-Tier Web Architecture Workshop.

The project is structured and deployed in a way that anyone can recreate the same architecture using the code and steps provided here.

<img width="1024" height="1536" alt="ChatGPT Image Nov 21, 2025, 04_00_10 PM" src="https://github.com/user-attachments/assets/6691068e-dba7-4336-a199-5e16705843fe" />

📌 Architecture Overview

Request Flow:

        Internet
        ⬇
        Public Application Load Balancer (ALB)
        ⬇
        Web Tier Auto Scaling Group (EC2 – Nginx + React)
        ⬇ /api
        Internal Application Load Balancer
        ⬇
        App Tier Auto Scaling Group (EC2 – Node.js + PM2)
        ⬇
        Amazon RDS (MySQL)

This design ensures:

✅ High Availability

✅ Scalability

✅ Secure network isolation

✅ Load balancing

✅ Fault tolerance

📁 Repository Structure



three-tier-aws-custom-repo/
│
├── frontend/
│   └── web-tier/       → React + Nginx
│
├── backend/
│   └── app-tier/        → Node.js + PM2
│
├── db/                  → Database notes / scripts
│
├── docs/
│   └── DEPLOYMENT_GUIDE.md
│
├── .gitignore
└── README.md             → You are here


🔹 Detailed Web Tier steps: frontend/web-tier/README.md
🔹 Detailed App Tier steps: backend/app-tier/README.md




🛠️ Technology Stack

    Cloud Provider: AWS

    Compute: Amazon EC2

    Load Balancer: Application Load Balancer (Public & Internal)

    Scaling: Auto Scaling Groups

    Database: Amazon RDS (MySQL)

    Web Server: Nginx

    Backend: Node.js + Express + PM2

    Frontend: React

    OS: Amazon Linux 2

    Network: VPC, Subnets, Route Tables, IGW, NAT Gateway



🌐 Network Architecture
VPC

    CIDR: 10.0.0.0/16

Subnets

    Public Subnets:

        public-subnet-1 → 10.0.1.0/24

        public-subnet-2 → 10.0.2.0/24
    
    Private Subnets:

        private-subnet-1 → 10.0.3.0/24

        private-subnet-2 → 10.0.4.0/24

    Gateways

        Internet Gateway (IGW) – for public subnet access

        NAT Gateway – for private subnet outbound access



    Route Tables

        Public Route Table:
        0.0.0.0/0 → Internet Gateway

        Private Route Table:
    0.0.0.0/0 → NAT Gateway



🔐 Security Groups
    
    Security Group	Purpose	Inbound Rules

        sg-public-alb	For Public ALB	HTTP 80 from 0.0.0.0/0
        sg-web-tier	For Web Tier EC2	HTTP 80 from sg-public-alb
        sg-internal-alb	For Internal ALB	HTTP 80 from sg-web-tier
        sg-app-tier	For App Tier EC2	TCP 3001 from sg-internal-alb
        sg-rds	For RDS	MySQL 3306 from sg-app-tier



This enforces strict layer-to-layer access only.




🗄️ Database Configuration

        Type: Amazon RDS (MySQL)

        DB Name: webappdb

        Placed in: Private Subnets

        Public Access: ❌ Disabled

        Access allowed only from App Tier SG



In backend/app-tier/DbConfig.js:

        DB_HOST = <YOUR_RDS_ENDPOINT>
        DB_USER = admin
        DB_PWD = your_password
        DB_DATABASE = webappdb
        DB_PORT = 3306




⚙️ Auto Scaling Setup
        
    Web Tier ASG
        
        Subnets: public-subnet-1 & public-subnet-2
        
        Target Group: web-tier-tg

        Desired: 2
        
        Min: 2
        
        Max: 4




    App Tier ASG

        Subnets: private-subnet-1 & private-subnet-2
        
        Target Group: app-tier-tg
        
        Desired: 2
        
        Min: 2
        
        Max: 4




Both include:

    Health checks

    Self-healing








✅ Final Test

Once everything is healthy:

    Go to EC2 → Load Balancers

    Copy Public ALB DNS Name

    Open in browser:

    http://<PUBLIC_ALB_DNS>


✅ Your application should load successfully.

<img width="1920" height="1080" alt="Screenshot 2025-11-21 155352" src="https://github.com/user-attachments/assets/87b1580a-8286-493c-ab31-4a9dddb961ec" />

