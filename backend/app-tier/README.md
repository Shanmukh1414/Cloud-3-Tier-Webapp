# App Tier

# App Tier – Backend (Node.js + PM2)

This folder is used for the **Application Tier (Backend)** of the 3-tier AWS architecture.

It runs on **EC2 instances in private subnet**, connected to:
- ✅ Internal Application Load Balancer
- ✅ Amazon RDS (MySQL)

Traffic Flow:

Web Tier → Internal ALB → App Tier (Node.js) → RDS

---

## What this folder contains

    - Node.js backend code
    - TransactionService.js
    - DbConfig.js (RDS connection)
    - index.js (main start file)

---

## How it connects to Internal ALB

Your **App Tier EC2 instance** is added to:

- **Target Group:** `app-tier-tg`
- **Connected to:** Internal Application Load Balancer



DbConfig.js

      Update it with your RDS details:
    
        module.exports = Object.freeze({
          DB_HOST : "<YOUR_RDS_ENDPOINT>",
          DB_USER : "admin",
          DB_PWD  : "yourpassword",
          DB_DATABASE : "webappdb",
          DB_PORT : 3306
        });
    
    
  Security Group rule in RDS:

      ✅ Allow MySQL (3306) only from App Tier Security Group

Steps to use this folder on App Tier EC2

   1. Install Node.js, Git and PM2

    curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -
    sudo yum install nodejs git -y
    sudo npm install pm2 -g


  
2. Clone your GitHub repo

        cd /home/ec2-user
        git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
        cd YOUR_REPO_NAME/backend/app-tier

  

3. Install dependencies

        npm install




4. Start app using PM2

        pm2 start index.js --name app-tier
        pm2 save
        pm2 status




5. Test locally

        curl http://localhost:3001
        If the app is running, you will see a response.

6. Add instance to Internal ALB Target Group

✅ Add the instance to app-tier target group
✅ Attach target group to Internal ALB
✅ Web tier will now communicate with it
