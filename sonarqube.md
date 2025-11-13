# 🚀 SonarQube Setup with Docker


### 🎯 Overview
SonarQube is an open-source platform for continuous **code quality and security analysis**, used to identify **bugs, vulnerabilities, and code smells**.

It easily integrates with Jenkins for automated code inspection during CI/CD pipelines.


### ⚙️ Prerequisites
Before starting, ensure you have:
- **AWS EC2 Instance:** Ubuntu 22.04 LTS or higher
- **Ports Open:**
  - `9000` → Sonarqube Web UI
  - `22` → SSH Access
- **Instance Specs:** Minimum 4 vCPU, 8GB RAM, 20GB disk
- **Privileges:** `sudo` access required
- **Internet Access:** For package downloads


### ⚡ Quick Start (Single-Container Setup)
The fastest way to run SonarQube on an Ubuntu instance using Docker:
```sh
# Update system packages
sudo apt update -y

# Install Docker Engine (if not already installed)
sudo apt install -y docker.io

# Run SonarQube container (LTS Community Edition)
sudo docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:lts-community

# Verify SonarQube container is running
sudo docker ps | grep sonarqube

# Check container logs
sudo docker logs -f sonarqube
```

### 🌐 Access SonarQube Web UI
Once the container is running:
- **Open in browser:** `http://<EC2-Public-IP>:9000`
- **Default Credentials:**
  - Username: `admin`
  - Password: `admin`
- **Recommendation:** Change the password immediately after first login.


Follow the SonarQube with PostgreSQL Setup Guide [here](/configuration/sonarqube-with-database.md)
