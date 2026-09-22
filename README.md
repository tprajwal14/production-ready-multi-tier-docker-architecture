🚀 Multi-Tier Web Application Deployment Using Docker Compose

A containerized multi-tier web application demonstrating NGINX + PHP-FPM + MySQL using Docker Compose. This project focuses on containerization, service isolation, Docker networking, persistent storage, and Linux/AWS EC2 deployment.

🏗️ Architecture

Internet
   │
 HTTP :80
   ▼
┌───────────────┐
│     NGINX     │
│     myweb     │
└───────┬───────┘
        │
 Frontend Network
        │
┌───────▼───────┐
│    PHP-FPM    │
│     myapp     │
│    PHP 8.3    │
└───────┬───────┘
        │
 Backend Network
        │
┌───────▼───────┐
│     MySQL     │
│     mydb      │
└───────────────┘

Volumes:
mydir  → Shared application files
mydata → Persistent MySQL data

🎯 Project Objectives

Deploy a multi-tier web application using Docker Compose

Containerize NGINX, PHP-FPM, and MySQL

Implement frontend and backend Docker network isolation

Use Docker volumes for persistent data

Verify container-to-container communication

Deploy and troubleshoot the application on Linux/AWS EC2

🛠️ Technology Stack

Technology

Purpose

Docker

Containerization

Docker Compose

Multi-container orchestration

NGINX

Web server / reverse proxy

PHP 8.3-FPM

Application runtime

MySQL

Database

Linux / Ubuntu

Server environment

AWS EC2

Cloud deployment

📁 Repository Structure

Linux/
├── Multi-Tier_Web_Application.yml
├── README.md
└── screenshots/
    ├── docker-compose-configuration.jpg
    ├── docker-containers-running.jpg
    ├── mysql-database-verification.jpg
    └── nginx-web-server.jpg

⚙️ Docker Compose Configuration

The application contains three services:

1. NGINX — myweb

Exposes HTTP port 80

Serves the web application

Connected to the frontend network

Shares application files through mydir

2. PHP-FPM — myapp

PHP 8.3-FPM

Connected to frontend and backend networks

Shares application files with NGINX

Handles PHP application processing

3. MySQL — mydb

Provides database storage

Connected to the backend network

Uses mydata for persistent storage

Initializes mydatabase

📦 Compose File

The complete configuration is available in:

Multi-Tier_Web_Application.yml

Example:

services:
  mydb:
    image: mysql
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: mydatabase
    networks:
      - backend
    volumes:
      - mydata:/var/lib/mysql

  myapp:
    image: php:8.3-fpm
    ports:
      - "9000:9000"
    networks:
      - frontend
      - backend
    volumes:
      - mydir:/var/www/html

  myweb:
    image: nginx
    ports:
      - "80:80"
    depends_on:
      - mydb
      - myapp
    networks:
      - frontend
    volumes:
      - mydir:/usr/share/nginx/html

networks:
  frontend:
  backend:

volumes:
  mydata:
  mydir:

Security: Never commit real passwords to GitHub. Use a .env file or secret-management solution.

MYSQL_ROOT_PASSWORD=your_secure_password

Add to .gitignore:

.env

🚀 Deployment

Clone the repository

git clone https://github.com/tprajwal14/dockerized-multi-tier-web-application.git
cd Linux

Create environment file

nano .env

Add:

MYSQL_ROOT_PASSWORD=your_secure_password

Start the application

sudo docker compose -f Multi-Tier_Web_Application.yml up -d

Verify containers

sudo docker ps

Expected services:

myweb
myapp
mydb

🌐 Application Verification

Open:

http://YOUR_EC2_PUBLIC_IP

Application flow:

Client → NGINX :80 → PHP-FPM :9000 → MySQL

🔍 Docker Verification

Networks

sudo docker network ls

Network design:

Frontend: NGINX ↔ PHP-FPM
Backend:  PHP-FPM ↔ MySQL

Inspect a network:

sudo docker network inspect <network-name>

Volumes

sudo docker volume ls

Expected volumes:

mydata
mydir

mydata stores MySQL data persistently.

mydir is shared between NGINX and PHP-FPM for application files.

Running Containers

sudo docker ps

Container

Role

Port

myweb

NGINX Web Server

80

myapp

PHP-FPM Application

9000

mydb

MySQL Database

3306

🗄️ MySQL Verification

Access MySQL:

sudo docker exec -it <mysql-container> mysql -u root -p

Check databases:

SHOW DATABASES;

Configured database:

mydatabase

🩺 Quick Health Checks

Task

Command

List containers

docker ps

List all containers

docker ps -a

List networks

docker network ls

Inspect network

docker network inspect <network>

List volumes

docker volume ls

Inspect volume

docker volume inspect <volume>

Compose status

docker compose -f Multi-Tier_Web_Application.yml ps

View logs

docker logs <container>

Compose logs

docker compose -f Multi-Tier_Web_Application.yml logs

Enter container

docker exec -it <container> /bin/bash

🧪 Troubleshooting

sudo docker ps -a
sudo docker compose -f Multi-Tier_Web_Application.yml ps
sudo docker compose -f Multi-Tier_Web_Application.yml logs
sudo docker compose -f Multi-Tier_Web_Application.yml logs -f
sudo docker compose -f Multi-Tier_Web_Application.yml restart
sudo docker compose -f Multi-Tier_Web_Application.yml down

down -v removes volumes and may delete persistent database data. Use it carefully.

📸 Project Screenshots

Docker Compose Configuration



Running Docker Containers



MySQL Database Verification



NGINX Web Server



🔐 Security Considerations

For a production-oriented deployment:

Store credentials in environment variables or a secret manager

Never commit .env files or real credentials

Restrict AWS Security Group rules

Avoid publicly exposing MySQL port 3306 unless required

Use HTTPS/TLS

Pin Docker image versions instead of unrestricted latest tags

Add container health checks

Apply least-privilege configuration

Implement logging and monitoring

📈 Future Enhancements

GitHub Actions CI/CD

Jenkins pipeline

AWS EC2 deployment automation

Terraform Infrastructure as Code

AWS Secrets Manager

HTTPS with Let's Encrypt

Prometheus and Grafana monitoring

Centralized logging

Docker image security scanning

Kubernetes deployment

Load balancing

🎓 DevOps Skills Demonstrated

Linux
Docker
Docker Compose
NGINX
PHP-FPM
MySQL
Docker Networking
Docker Volumes
Service Isolation
Container Troubleshooting
AWS EC2
Application Deployment
