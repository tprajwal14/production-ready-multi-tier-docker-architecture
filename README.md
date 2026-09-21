🚀 Multi-Tier Web Application Deployment Using Docker Compose

A containerized multi-tier web application demonstrating NGINX + PHP-FPM + MySQL using Docker Compose. The project focuses on containerization, service isolation, Docker networking, persistent storage, and Linux/AWS EC2 deployment.

🏗️ Architecture

                         Internet
                            │
                         HTTP :80
                            │
                            ▼
                    ┌─────────────────┐
                    │      NGINX      │
                    │     myweb       │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │    PHP-FPM      │
                    │     myapp       │
                    │    PHP 8.3      │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │      MySQL      │
                    │      mydb       │
                    └─────────────────┘

Frontend Network: NGINX ↔ PHP-FPM
Backend Network:  PHP-FPM ↔ MySQL

Volumes:
mydir  → shared application files
mydata → persistent MySQL data

🎯 Project Objectives

Deploy a multi-tier web application with Docker Compose

Containerize NGINX, PHP-FPM, and MySQL

Implement frontend/backend Docker network isolation

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

📦 Project Structure

dockerized-multi-tier-web-application/
│
├── docker-compose.yml
├── nginx/
│   └── default.conf
├── app/
│   ├── index.php
│   └── ...
├── screenshots/
│   ├── docker-compose-configuration.png
│   ├── docker-containers-running.png
│   ├── nginx-web-server.png
│   └── mysql-database-verification.png
└── README.md

⚙️ Docker Compose Configuration

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

Security: Do not commit real passwords to GitHub. Store credentials in .env and add .env to .gitignore.

Example .env:

MYSQL_ROOT_PASSWORD=your_secure_password

🚀 Deployment

1. Clone the repository

git clone https://github.com/tprajwal14/dockerized-multi-tier-web-application.git
cd dockerized-multi-tier-web-application

2. Create the environment file

nano .env

Add:

MYSQL_ROOT_PASSWORD=your_secure_password

3. Start the application

sudo docker compose up -d

4. Verify containers

sudo docker ps

Expected services:

myweb
myapp
mydb

🌐 Application Verification

Open the EC2 public IP in a browser:

http://YOUR_EC2_PUBLIC_IP

Expected application flow:

Client
  ↓
NGINX :80
  ↓
PHP-FPM :9000
  ↓
MySQL

🔍 Docker Verification

List Docker Networks

sudo docker network ls

The application uses:

frontend → NGINX ↔ PHP-FPM
backend  → PHP-FPM ↔ MySQL

Inspect a Network

sudo docker network inspect <network-name>

List Docker Volumes

sudo docker volume ls

Expected volumes include:

mydata
mydir

Inspect a Volume

sudo docker volume inspect <volume-name>

List Running Containers

sudo docker ps

View Container Logs

sudo docker logs <container-name>

🗄️ MySQL Verification

Access MySQL from the database container:

sudo docker exec -it <mysql-container> mysql -u root -p

Check databases:

SHOW DATABASES;

The configured database is:

mydatabase

🧪 Useful Troubleshooting Commands

Check all containers

sudo docker ps -a

Check Compose services

sudo docker compose ps

View Compose logs

sudo docker compose logs

Follow logs

sudo docker compose logs -f

Restart services

sudo docker compose restart

Stop services

sudo docker compose down

Stop and remove volumes

sudo docker compose down -v

Use down -v carefully because it removes Docker volumes and can delete persistent database data.

Enter a container

sudo docker exec -it <container-name> /bin/bash

🔐 Security Considerations

For a production deployment, consider:

Store credentials in environment variables or a secret manager

Never commit .env files or real credentials

Restrict AWS Security Group rules

Avoid publicly exposing MySQL port 3306 unless required

Use HTTPS/TLS

Pin Docker image versions instead of relying on latest

Add Docker health checks

Use least-privilege container configuration

Implement logging and monitoring

📸 Project Screenshots

Docker Compose Configuration



Running Docker Containers



NGINX Web Server



MySQL Database Verification



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
