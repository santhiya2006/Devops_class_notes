# 🚀 Ubuntu DevOps Setup & CI/CD Project

Complete end‑to‑end DevOps environment setup on Ubuntu including:

- SSH Configuration
- Git & GitHub Setup
- Docker Installation
- Node.js Environment
- Nginx Web Server
- Jenkins CI/CD Pipeline
- Terraform (Infrastructure as Code)
- Ansible (Configuration Management)

---

# 📌 Project Overview

This project demonstrates a full DevOps workflow:

1. Configure Ubuntu server
2. Secure access using SSH
3. Setup Git & GitHub authentication
4. Containerize application using Docker
5. Setup Jenkins for CI/CD automation
6. Provision infrastructure using Terraform
7. Configure servers using Ansible

---

# 🖥️ 1. System Update

```bash
sudo apt update
sudo apt upgrade -y
```

---

# 🔐 2. SSH Setup

Install SSH server:

```bash
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

Allow SSH in firewall:

```bash
sudo ufw allow ssh
```

Check network:

```bash
ip a
ping google.com
```

---

# 🧰 3. Git & GitHub Setup

## Install Git

```bash
sudo apt install git openssh-client -y
git --version
```

## Configure Git

```bash
git config --global user.name "spiicez21"
git config --global user.email "yuga.bharathijai2106@gmail.com"
git config --list
```

## Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "yuga.bharathijai2106@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

Add public key to GitHub.

Test connection:

```bash
ssh -T git@github.com
```

---

# 🐳 4. Docker Installation

## Install Dependencies

```bash
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
```

## Add Docker Repository

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Install Docker Engine

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Verify Docker

```bash
sudo docker run hello-world
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

---

# 🟢 5. Node.js Installation

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
sudo apt install -y build-essential
node -v
npm -v
```

---

# 🌐 6. Nginx Installation

```bash
sudo apt install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

Allow firewall:

```bash
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

---

# 📦 7. Docker Deployment (Portfolio App)

## Clone Repository

```bash
git clone https://github.com/spiicez21/SPiceZ-Portfolio.git
cd SPiceZ-Portfolio
npm install
```

## Build Application

```bash
npm run build
```

## Run Container

```bash
docker rm -f portfolio

docker run -d -p 8080:80 \
  --name portfolio \
  -v $(pwd)/dist:/usr/share/nginx/html \
  nginx
```

Access:

```
http://server-ip:8080
```

---

# ⚙️ 8. Jenkins Installation

## Install Java

```bash
sudo apt install openjdk-17-jdk -y
java -version
```

## Add Jenkins Repository

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | \
sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

## Install Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

Get admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# 🔄 9. Jenkins CI/CD Pipeline

Pipeline Stages:

1. Clean Workspace
2. Clone Repository
3. Install Dependencies
4. Build Application
5. Build Docker Image
6. Deploy Container

Example Docker deployment:

```bash
docker rm -f portfolio-container

docker run -d -p 3000:80 \
  --name portfolio-container \
  portfolio-image
```

Pipeline Result:

```
Finished: SUCCESS
```

---

# 🏗️ 10. Terraform (Infrastructure as Code)

## Install Terraform

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
sudo apt update
sudo apt install terraform
terraform --version
```

## Terraform Workflow

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```

---

# 🤖 11. Ansible (Configuration Management)

## Install Ansible

```bash
sudo apt update
sudo apt install ansible -y
ansible --version
```

## Inventory File Example

```ini
[web]
server1 ansible_host=35.85.32.142 ansible_user=ubuntu ansible_ssh_private_key_file=~/Downloads/openssh.pem
```

## Run Playbook

```bash
ansible-playbook -i inventory.ini setup.yml
```

This will:

- Update the package cache
- Install Docker
- Install Nginx
- Start and enable Nginx service
- Apply configuration in an automated and repeatable way

---

## 🔎 Verify Deployment

Check Nginx service status remotely:

```bash
ansible all -i inventory.ini -a "systemctl status nginx"
```

Check Docker installation:

```bash
ansible all -i inventory.ini -a "docker --version"
```

Test web server in browser:

```
http://YOUR_PUBLIC_IP
```

---

## 🛠️ Useful Ansible Ad-hoc Commands

Check server uptime:

```bash
ansible all -i inventory.ini -a "uptime"
```

Install Git on remote server:

```bash
ansible all -i inventory.ini -m apt -a "name=git state=present" --become
```

Reboot remote server:

```bash
ansible all -i inventory.ini -a "reboot" --become
```

Check disk usage:

```bash
ansible all -i inventory.ini -a "df -h"
```

---

## 🔐 Security Best Practices (AWS + Ansible)

✔ Restrict SSH access in Security Group (Port 22 → My IP only)  
✔ Use proper key permissions (`chmod 400 key.pem`)  
✔ Do not store private keys inside Git repositories  
✔ Use IAM Roles instead of hardcoding AWS credentials  
✔ Regularly rotate SSH keys  

---

## 📂 Recommended Ansible Project Structure

```
ansible/
│── inventory.ini
│── setup.yml
│── roles/
│   ├── docker/
│   ├── nginx/
│   └── common/
```

Using roles makes your project modular and production‑ready.

---

## 🚀 Expected Result

After running the playbook:

✔ SSH automation working  
✔ Remote server configured automatically  
✔ Docker installed  
✔ Nginx running  
✔ Infrastructure managed as code  

Ansible now acts as your **Configuration Management tool** in the DevOps pipeline.


---

# 🧱 DevOps Architecture Flow

```
Developer → GitHub → Jenkins → Docker Build → Container Deployment → Server
                               ↓
                           Terraform
                               ↓
                           AWS Infra
                               ↓
                           Ansible Config
```

---

# 📚 Technologies Used

- Ubuntu
- Git & GitHub
- Docker
- Node.js
- Nginx
- Jenkins
- Terraform
- Ansible
- AWS

---

# ✅ Final Result

✔ Automated CI/CD Pipeline  
✔ Containerized Application  
✔ Infrastructure as Code  
✔ Automated Server Configuration  
✔ Production Deployment Ready  

---

# 👨‍💻 Author

**spiicez21**  
DevOps & Cloud Enthusiast 🚀


---

# 🚀 Jenkins CI/CD Setup and Pipeline Documentation

## 🔐 Secure Jenkins (Production Recommendations)

✔ Change default admin credentials  
✔ Enable HTTPS using Nginx reverse proxy  
✔ Restrict port 8080 using firewall  
✔ Enable Role-Based Access Control (RBAC)  
✔ Regularly update Jenkins and plugins  

---

## 🌍 Configure Nginx Reverse Proxy for Jenkins

```bash
sudo apt install nginx -y
```

Create configuration:

```bash
sudo nano /etc/nginx/sites-available/jenkins
```

Example config:

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/jenkins /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

---

## 🧪 GitHub Webhook for Auto Trigger

Payload URL:

```
http://YOUR_SERVER_IP:8080/github-webhook/
```

This enables automatic pipeline execution on every push.

---

## 📦 Example Multi-stage Dockerfile

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 📊 Monitoring Jenkins

```bash
sudo systemctl status jenkins
sudo journalctl -u jenkins -f
```

Monitor containers:

```bash
docker ps
docker stats
```

---

## 🎯 Final Outcome

✔ Fully automated CI/CD  
✔ Dockerized deployment  
✔ Production-ready automation  
✔ Scalable DevOps workflow  

Jenkins acts as the **Automation Engine** of the DevOps lifecycle.

