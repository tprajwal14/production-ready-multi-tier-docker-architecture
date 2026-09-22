🚀 Multi-Tier Web Application Deployment Using Docker Compose

A professional multi-tier web application deployment using NGINX, PHP-FPM, MySQL, Docker Compose, Linux, and AWS EC2.

🏗️ Architecture
                         Internet
                            |
                         HTTP :80
                            |
                            v
                  +-------------------+
                  |       NGINX       |
                  |      myweb        |
                  |   Web Server      |
                  +---------+---------+
                            |
                     Frontend Network
                            |
                            v
                  +-------------------+
                  |     PHP-FPM       |
                  |      myapp        |
                  |      PHP 8.3      |
                  | Application Layer |
                  +---------+---------+
                            |
                     Backend Network
                            |
                            v
                  +-------------------+
                  |       MySQL       |
                  |       mydb        |
                  |   Database Layer  |
                  +-------------------+

Docker Volumes
-----------------------------
mydir  -> Shared application files
mydata -> Persistent MySQL data

Request Flow
Client
  |
  v
NGINX :80
  |
  v
PHP-FPM :9000
  |
  v
MySQL :3306
🎯 Project Objectives
Deploy a multi-tier web application using Docker Compose
Containerize the web, application, and database layers
Implement Docker network isolation
Use Docker volumes for persistent storage
Demonstrate container-to-container communication
Deploy and manage the application on Linux/Ubuntu
Deploy and troubleshoot the environment on AWS EC2
Practice Docker and Linux administration
🛠️ Technology Stack
Technology	Purpose
Docker	Application containerization
Docker Compose	Multi-container orchestration
NGINX	Web server
PHP 8.3-FPM	Application runtime
MySQL	Relational database
Linux / Ubuntu	Server operating system
AWS EC2	Cloud deployment
Git / GitHub	Version control
📁 Repository Structure
Linux/
├── Multi-Tier_Web_Application.yml
├── README.md
└── screenshots/
    ├── docker-compose-configuration.jpg
    ├── docker-containers-running.jpg
    ├── mysql-database-verification.jpg
    └── nginx-web-server.jpg
🐳 Docker Services
NGINX — myweb
Web server
Port 80
Connected to the frontend network
Uses the mydir volume
Handles incoming HTTP requests
PHP-FPM — myapp
Application runtime
PHP 8.3-FPM
Port 9000
Connected to frontend and backend
Uses the mydir volume
MySQL — mydb
Database layer
Connected to the backend network
Uses the mydata volume
Creates the mydatabase database
🌐 Docker Network Architecture
Network	Services	Purpose
frontend	NGINX + PHP-FPM	Web-to-application communication
backend	PHP-FPM + MySQL	Application-to-database communication
NGINX
  |
  | frontend
  |
PHP-FPM
  |
  | backend
  |
MySQL
💾 Docker Volumes
Volume	Purpose
mydir	Shared application files
mydata	Persistent MySQL data
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
🔐 Environment Variables

Create a .env file:

MYSQL_ROOT_PASSWORD=your_secure_password

Add .env to .gitignore:

.env

Never commit real passwords, API keys, private keys, or other secrets to GitHub.

🚀 Deployment
Prerequisites
Docker
Docker Compose
Git
Ubuntu/Linux server
AWS EC2 instance
EC2 Security Group allowing HTTP port 80

Verify installation:

docker --version
docker compose version
Clone Repository
git clone https://github.com/tprajwal14/dockerized-multi-tier-web-application.git
cd dockerized-multi-tier-web-application

If the project is inside the Linux directory:

cd Linux
Start Application
sudo docker compose -f Multi-Tier_Web_Application.yml up -d

Check containers:

sudo docker ps
🔍 Verify Deployment
Container Status
sudo docker ps
sudo docker ps -a
Docker Networks
sudo docker network ls
sudo docker network inspect <network-name>
Docker Volumes
sudo docker volume ls
sudo docker volume inspect <volume-name>
🌐 NGINX Verification

Open:

http://YOUR_EC2_PUBLIC_IP

Request flow:

Browser
   |
   v
EC2 Public IP :80
   |
   v
NGINX
🗄️ MySQL Verification

Connect to MySQL:

sudo docker exec -it <mysql-container> mysql -u root -p

Check databases:

SHOW DATABASES;

Expected database:

mydatabase
🧰 Useful Docker Commands
Containers
sudo docker ps
sudo docker ps -a
sudo docker start <container>
sudo docker stop <container>
sudo docker restart <container>
sudo docker rm <container>
Logs
sudo docker logs <container>
sudo docker logs -f <container>
Container Shell
sudo docker exec -it <container> /bin/bash
Docker Compose
sudo docker compose -f Multi-Tier_Web_Application.yml ps
sudo docker compose -f Multi-Tier_Web_Application.yml logs
sudo docker compose -f Multi-Tier_Web_Application.yml logs -f
sudo docker compose -f Multi-Tier_Web_Application.yml restart
sudo docker compose -f Multi-Tier_Web_Application.yml down
🛠️ Troubleshooting

Check containers:

sudo docker ps -a

Check Compose:

sudo docker compose -f Multi-Tier_Web_Application.yml ps

Check logs:

sudo docker compose -f Multi-Tier_Web_Application.yml logs

Follow logs:

sudo docker compose -f Multi-Tier_Web_Application.yml logs -f

Restart services:

sudo docker compose -f Multi-Tier_Web_Application.yml restart

Check networks:

sudo docker network ls
sudo docker network inspect <network-name>

Check volumes:

sudo docker volume ls
sudo docker volume inspect <volume-name>

Warning: docker compose down -v removes Docker volumes and may permanently delete MySQL data.

📸 Screenshots
Docker Compose Configuration

Docker Containers Running

NGINX Web Server

MySQL Database Verification

🔒 Security Considerations
Do not commit .env files
Never expose production credentials
Restrict AWS Security Group inbound rules
Avoid publicly exposing MySQL port 3306
Use HTTPS/TLS for public applications
Pin Docker image versions
Use least-privilege database users
Regularly update container images
Scan images for vulnerabilities
Implement application and infrastructure monitoring
Use AWS Secrets Manager or another secret-management solution for production
🔮 Future Enhancements
GitHub Actions CI/CD
Jenkins CI/CD
Amazon ECR
Automated AWS EC2 deployment
Terraform Infrastructure as Code
AWS Secrets Manager
HTTPS with Let's Encrypt
Prometheus and Grafana
Centralized logging
Container vulnerability scanning
Docker health checks
Load balancing
Kubernetes
Auto Scaling
Infrastructure automation
💡 DevOps Skills Demonstrated
Linux Administration
Docker
Docker Compose
NGINX
PHP-FPM
MySQL
Docker Networking
Docker Volumes
Container Troubleshooting
AWS EC2
Git
GitHub
Application Deployment
Production Support
Cloud Infrastructure
✨ Project Highlights
Multi-tier application architecture
Containerized NGINX web server
PHP 8.3-FPM application runtime
MySQL database container
Frontend and backend network separation
Persistent Docker volumes
Linux-based deployment
AWS EC2 deployment
Docker troubleshooting
Git and GitHub project management

👨‍💻 Author

Prajwal Take

AWS | DevOps | Linux | Docker | Cloud
