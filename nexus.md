# 🚀 Nexus Repository Manager Setup with Docker

Nexus Repository Manager 3 is an artifact repository for storing and managing Maven builds, Docker images, npm packages, and other dependencies.

It acts as a central hub for your CI/CD pipelines, caching and distributing artifacts securely.


## ⚙️ Prerequisites
Before starting, ensure you have:
- **AWS EC2 Instance:** Ubuntu 22.04 LTS or higher
- **Ports Open:**
  - `8081` → Nexus Web UI
  - `22` → SSH Access
- **Instance Specs:** Minimum 4 vCPU, 8GB RAM, 20GB disk
- **Privileges:** `sudo` access required
- **Internet Access:** For package downloads


## ⚡ Quick Start (Single Container)
The fastest way to get Nexus running on an Ubuntu server:
```sh
# Update package list
sudo apt update -y

# Install Docker
sudo apt install -y docker.io

# Run Nexus container
sudo docker run -d \
  --name nexus \
  -p 8081:8081 \
  -v nexus-data:/nexus-data \
  sonatype/nexus3

# Check container status
sudo docker ps | grep nexus

# View logs
sudo docker logs -f nexus
```

## 🌐 Access Nexus Web UI
Once the container is running:
- **Open in browser:** `http://<EC2-Public-IP>:8081`
- **Default Credentials:**
  - Username: `admin`
  - Password: from container
  ```sh
  sudo docker exec -it nexus cat /nexus-data/admin.password
  ```
- **Recommendation:** Change the password immediately after first login.


## ⚙️ Configure Repositories
After login, create or verify these repositories:

**1. Docker Registry (Hosted):**
   - Navigate to **Settings → Repositories → Create repository**
   - Select docker (hosted)
   - Name: `docker-hosted`
   - HTTP Port: `8082`
   - Enable Docker V1 API: ☑️

**2. Maven Releases:**
   - Type: maven2 (hosted)
   - Name: `maven-releases`
   - Version policy: `Release`

**3. Maven Snapshots:**
   - Type: maven2 (hosted)
   - Name: `maven-snapshots`
   - Version policy: `Snapshot`
