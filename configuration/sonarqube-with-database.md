# SonarQube Setup (With Database)

For persistent data and better performance:
```sh
# Create network
sudo docker network create sonarnet

# Run PostgreSQL
sudo docker run -d \
  --name sonarqube_db \
  --network sonarnet \
  -e POSTGRES_USER=sonar \
  -e POSTGRES_PASSWORD=sonar \
  -e POSTGRES_DB=sonarqube \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:latest

# Run SonarQube
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

### Generate Authentication Token

**For Jenkins integration:**
1. Login to SonarQube web UI
2. Go to Administration → Security → Users
3. Click Tokens icon for admin user
4. Generate token:
   - Name: jenkins
   - Type: Global Analysis Token
5. Copy and save the token securely


### Verify Installation
```sh
# Check container status
sudo docker ps | grep sonarqube

# View logs
sudo docker logs sonarqube

# Access web UI
curl http://<server_ip>:9000
```


### Common Commands
```sh
# Start
sudo docker start sonarqube

# Stop
sudo docker stop sonarqube

# Restart
sudo docker restart sonarqube

# View logs
sudo docker logs -f sonarqube

# List volumes
docker volume ls | grep -E 'sonar'

# Backup volumes
# Backup Nexus data volume
sudo docker run --rm \
  -v nexus-data:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/nexus-backup.tar.gz /data

# Remove containers and volumes
sudo docker rm -f sonarqube sonarqube_db
sudo docker volume rm sonarqube-data postgres-data
```

### Data Persistence
Sonar use Docker volumes for data persistence
```sh
# List volumes
sudo docker volume ls | grep -E 'sonar'
```

**1. Single Container Setup:**
```sh
# Backup SonarQube data volume
docker run --rm \
  -v sonarqube-data:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/sonarqube-backup.tar.gz /data

```

**2. With Database (Production Setup):**
```sh
# Backup PostgreSQL data
sudo docker run --rm \
  -v postgres-data:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/sonarqube-db-backup.tar.gz /data

# Backup SonarQube volumes
sudo docker run --rm \
  -v sonarqube-data:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/sonarqube-data-backup.tar.gz /data

sudo docker run --rm \
  -v sonarqube-extensions:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/sonarqube-extensions-backup.tar.gz /data
```