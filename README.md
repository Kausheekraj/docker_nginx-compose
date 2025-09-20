# docker_nginx-compose

# 🚀 Docker Task-  Custom Nginx Deployment

This project builds and deploys a custom Nginx container using Docker Compose on an AWS EC2 instance. It demonstrates containerization, image customization, volume mounting, and automated provisioning via EC2 user data.

---

## 🧰 Tech Stack

- **AWS EC2** (Ubuntu 22.04)
- **Docker**
- **Docker Compose v2**
- **Custom Docker Image**
- **Bind Mount Volume**

---


## 🛠 Setup Instructions

### 1. Launch EC2 Instance
- Use Ubuntu 22.04 AMI
- Open ports 22 (SSH) and 80 (HTTP) in the security group
- Paste the user data script during launch (see below)

### 2. EC2 User Data Script

#!/bin/bash
set -e
apt-get update -y
apt-get install -y docker.io curl
systemctl enable docker
systemctl start docker
curl -L "https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

mkdir -p /home/ubuntu/docker_nginx/html
echo "<h1>Hi from Custom Nginx</h1>" > /home/ubuntu/docker_nginx/html/index.html

cat > /home/ubuntu/docker_nginx/Dockerfile <<EOF
FROM nginx:latest
COPY html/ /usr/share/nginx/html
EOF

cat > /home/ubuntu/docker_nginx/docker-compose.yml <<EOF
version: '3.8'
services:
  nginx:
    build: .
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
EOF

cd /home/ubuntu/docker_nginx
docker-compose up --build -d


✅ Verification Commands
docker --version
docker compose version
docker ps
tree /home/ubuntu/docker_nginx
cat /home/ubuntu/docker_nginx/html/index.html
cat /home/ubuntu/docker_nginx/Dockerfile
cat /home/ubuntu/docker_nginx/docker-compose.yml

📌 Reference 
 - Dockerdocs
 - Nginx Documentation
 - Guvi Devops course

📌 Notes
- Volume bind mount is set to /usr/share/nginx/html to serve live content.
- EC2 provisioning is automated using cloud-init via user data.
- This project builds on previous Docker and Linux tasks, adding image customization and deployment automation.


