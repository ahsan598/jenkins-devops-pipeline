# 🚀 Jenkins CI/CD Server Setup on Ubuntu (Quick Guide)


### 🎯 Overview
A simplified guide to set up a **Jenkins CI/CD server** on an Ubuntu AWS instance, integrated with **Docker** for containerized builds and **Trivy** for vulnerability scanning.

This setup includes:
- **Jenkins** – CI/CD automation server with Java 21  
- **Docker** – Container engine for building and deploying applications  
- **Trivy** – Security vulnerability scanner for container images 


### ⚙️ Prerequisites
Before starting, ensure you have:
- **AWS EC2 Instance:** Ubuntu 22.04 LTS or higher
- **Ports Open:**
  - `8080` → Jenkins Web UI
  - `22` → SSH Access
- **Instance Specs:** Minimum 2 vCPU, 4GB RAM, 20GB disk
- **Privileges:** `sudo` access required
- **Internet Access:** For package downloads


## 🛠️ Installation Steps

### Step-1: Install Jenkins Server
```sh
sudo apt update
sudo apt install -y openjdk-21-jre-headless

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update && sudo apt install -y jenkins
sudo systemctl enable --now jenkins
```

**Verify Jenkins:**
```sh
# Check Jenkins service status (look for "active (running)")
sudo systemctl status jenkins --no-pager

# Verify Java version
java -version
```


### Step-2: Docker Installation on Jekins server
```sh
sudo apt install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Verify Docker:**
```sh
# Verify Docker installation
sudo docker --version

# Verify by running the hello-world image
sudo docker run hello-world
```

**Allow Jenkins to use Docker:**
```sh
# Add Jenkins system user to docker group
sudo usermod -aG docker jenkins

# Restart Jenkins to apply group changes
sudo systemctl restart jenkins                    
```

> **Note:** Using `$USER` here will not work for Jenkins pipelines because Jenkins runs under the `jenkins` system user. Always use `jenkins` user for group addition.


### Step-3 Trivy Installation on Jenkins server
```sh
sudo apt install -y wget apt-transport-https gnupg lsb-release

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list

sudo apt update && sudo apt install -y trivy
```

**Verify Trivy:**
```sh
# Verify Trivy installation
trivy --version
```

### 🌐 Access Jenkins Web UI

- **Open in browser:** `http://<EC2-Public-IP>:8080`
- **Retrieve admin password:**
```txt
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

### 🔧 Troubleshooting (Common Issues and Solutions)

**1. Jenkins not starting**
```sh
# Check logs
sudo journalctl -u jenkins -f

# Restart service
sudo systemctl restart jenkins

# Check port availability
sudo netstat -tulnp | grep 8080

# Check system resources
free -h
df -h

# Monitor Jenkins process
ps aux | grep jenkins

# View Jenkins logs
sudo tail -f /var/log/jenkins/jenkins.log

# Check all services status
sudo systemctl status jenkins docker
```

**2. Docker Permission Denied**
```sh
# Re-add users to docker group
sudo usermod -aG docker $USER
sudo usermod -aG docker jenkins

# Restart Docker
sudo systemctl restart docker

# Restart Jenkins
sudo systemctl restart jenkins
```

**3. Trivy Scan Fails in Jenkins**
```sh
# Verify Trivy is executable by Jenkins
sudo -u jenkins trivy --version

# Check Trivy path in Jenkins Tools configuration
which trivy

# Update Trivy database
trivy image --download-db-only
```


### 🧹 Cleanup (Optional)
To remove Jenkins and related tools:
```sh
# Stop services
sudo systemctl stop jenkins docker

# Remove packages
sudo apt remove --purge jenkins docker-ce docker-ce-cli containerd.io -y

# Remove data directories (CAUTION: Deletes all data)
sudo rm -rf /var/lib/jenkins
sudo rm -rf /var/lib/docker

# Remove Trivy
sudo rm /usr/local/bin/trivy
```

Follow the Post-Installation Jenkins Configuration Guide [here](/jenkins-configuration.md).


---

### 📚 References

- [Install Jenkins](https://www.jenkins.io/doc/book/installing/)
- [Install Docker](https://docs.docker.com/engine/install/)
- [Install Trivy](https://trivy.dev/docs/latest/getting-started/installation/)
