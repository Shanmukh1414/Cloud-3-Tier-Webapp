# Web Tier – Frontend (Nginx + React)

This folder is used for the **web tier** of the 3-tier architecture.

It runs on **EC2 (Amazon Linux 2)** and is connected to a **Public Load Balancer**.

---

## What this folder contains
        
        - React frontend
        - Nginx configuration
        - /health endpoint for load balancer
        - /api proxy to Internal Load Balancer

---

## Steps to use in AWS EC2

1. Install required packages



            sudo yum update -y
            sudo yum install nginx git -y
            curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -
            sudo yum install nodejs -y

2. Clone your GitHub repo


            cd /home/ec2-user
            git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
            cd YOUR_REPO_NAME/frontend/web-tier



3. Build the React app

            npm install
            npm run build



4. Set Nginx config

            Edit:
            sudo vi /etc/nginx/nginx.conf
        
            Update /api/ section to use your Internal ALB DNS name:
        
            location /api/ {
                proxy_pass http://<INTERNAL_ALB_DNS_NAME>;
            }



Make sure root path is:

        root /home/ec2-user/YOUR_REPO_NAME/frontend/web-tier/build;




5. Start Nginx

            sudo systemctl start nginx
            sudo systemctl enable nginx



6. Test from EC2

            curl http://localhost/health


You should see:

           Web Tier Health Check
