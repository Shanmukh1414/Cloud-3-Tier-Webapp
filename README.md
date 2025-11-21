# Three-Tier AWS Web Application – Custom Implementation

This repository is my custom implementation of a **three-tier web application on AWS**, inspired by the official AWS workshop:

> Original source: `aws-samples/aws-three-tier-web-architecture-workshop`

I used the workshop code as a base and reorganised it into a simpler, portfolio-friendly layout with three main folders: **frontend**, **backend**, and **db**.

## 📁 Project Structure

```text
frontend/
  └── web-tier/      # React app & web-tier config

backend/
  └── app-tier/      # Node.js backend (TransactionService, DbConfig, etc.)

db/
  └── README.md      # Notes / space for DB scripts (RDS MySQL)
```

- **Frontend**: React single-page application that talks to the backend via `/api/...`.
- **Backend**: Node.js + Express/PM2 service that connects to an Amazon RDS MySQL database.
- **Database**: RDS MySQL deployed in a private subnet, accessed only by the app tier.

## 🚀 High-Level Architecture

- **Public ALB** → routes internet traffic to Web Tier (Auto Scaling Group).
- **Web Tier (Nginx + React)** → serves frontend and proxies `/api` to Internal ALB.
- **Internal ALB** → load balances requests to App Tier (Auto Scaling Group).
- **App Tier (Node.js + PM2)** → handles business logic and DB queries.
- **Amazon RDS MySQL** → persistent data storage, reachable only from App Tier security group.

## 🔧 Running Locally (Dev)

### Frontend

```bash
cd frontend/web-tier
npm install
npm start        # or npm run build && serve -s build
```

### Backend

```bash
cd backend/app-tier
npm install

# configure DbConfig.js or .env for your local / RDS database
pm2 start index.js --name app-tier
```

## ☁️ Deployment Notes (AWS)

In AWS, I deployed this using:

- VPC with public and private subnets.
- Public ALB in public subnets → Web Tier Auto Scaling Group.
- Internal ALB in private subnets → App Tier Auto Scaling Group.
- RDS MySQL in private subnets.
- Security groups to tightly control traffic between each tier.

This repo is **execution-ready** as a code base and can be connected to any AWS infrastructure that follows the above pattern.
