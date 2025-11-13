# SonarQube Setup with PostgreSQL (Docker-Based)


### 🎯 Overview
SonarQube with PostgreSQL provides **persistent storage, better performance, and CI/CD integration** with Jenkins.


### ⚙️ Setup Steps
```sh
# Create dedicated Docker network
sudo docker network create sonarnet

# Run PostgreSQL container
sudo docker run -d \
  --name sonarqube_db \
  --network sonarnet \
  -e POSTGRES_USER=sonar \
  -e POSTGRES_PASSWORD=sonar \
  -e POSTGRES_DB=sonarqube \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:latest

# Run SonarQube container (LTS Community)
sudo docker run -d \
  --name sonarqube \
  --network sonarnet \
  -p 9000:9000 \
  -e sonar.jdbc.url=jdbc:postgresql://sonarqube_db:5432/sonarqube \
  -e sonar.jdbc.username=sonar \
  -e sonar.jdbc.password=sonar \
  -v sonarqube-data:/opt/sonarqube/data \
  -v sonarqube-logs:/opt/sonarqube/logs \
  -v sonarqube-extensions:/opt/sonarqube/extensions \
  sonarqube:lts-community
```

### Verify Installation
```sh
# Check container status
sudo docker ps | grep sonarqube

# View logs
sudo docker logs -f sonarqube
```


### 🌐 Access SonarQube Web UI
- **Open in browser:** `http://<EC2-Public-IP>:9000`
- **Default Login:** `admin / admin`
- Change password on first login.


### 🔑 Generate Authentication Token (for Jenkins)
1. Login → **Administration** → **Security** → **Users**
2. Click token icon for `admin` user
3. Create new token (e.g., `jenkins-token`)
4. Save it securely — used in Jenkins integration.


## 💾 Backup & Restore
**Backup:**
```sh
# Backup PostgreSQL data
sudo docker run --rm -v postgres-data:/data -v $(pwd):/backup ubuntu \
  tar czf /backup/sonar-db-backup.tar.gz /data

# Backup SonarQube volumes
sudo docker run --rm -v sonarqube-data:/data -v $(pwd):/backup ubuntu \
  tar czf /backup/sonar-data-backup.tar.gz /data
sudo docker run --rm -v sonarqube-extensions:/data -v $(pwd):/backup ubuntu \
  tar czf /backup/sonar-extensions-backup.tar.gz /data
```

**Restore:**
Extract tar files back into respective Docker volumes.


### 🧰 Useful Commands
```sh
# Start / Stop containers
sudo docker start sonarqube sonarqube_db
sudo docker stop sonarqube sonarqube_db

# Check volumes
sudo docker volume ls | grep sonar

# Remove setup (cleanup)
sudo docker rm -f sonarqube sonarqube_db
sudo docker volume rm sonarqube-data sonarqube-extensions postgres-data
```


### ✅ Summary
- Runs SonarQube with PostgreSQL backend for persistence
- Data stored safely in Docker volumes
- Ready for Jenkins integration via token authentication
- Recommended image: sonarqube:lts-community
