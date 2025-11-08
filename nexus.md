# Nexus Repository Manager Setup with Docker
Nexus is an artifact repository manager for storing build artifacts, Docker images, and dependencies.


### Quick Start
Fastest way to get Nexus running:
```sh
# Update package list
sudo apt update -y

# Install Docker
sudo apt install docker.io -y

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

### Access Nexus:
- URL: `http://<server_ip>:8081`
- Username: admin
- Password: See below

#### Get Initial Admin Password

**Option 1: From Logs**
```sh
sudo docker logs nexus 2>&1 | grep -i password
```

**Option 2: From Container**
```sh
# Access container
sudo docker exec -it nexus cat /nexus-data/admin.password
```

### First Login:

1. Open `http://<server_ip>:8081`
2. Click **Sign In**
3. Username: `admin`
4. Password: (from above command)
5. Follow setup wizard to change password


### Configure Repositories

#### After login, setup these repositories:**

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


### Common Commands
```sh
# Start
sudo docker start nexus

# Stop
sudo docker stop nexus

# Restart
sudo docker restart nexus

# View logs
sudo docker logs -f nexus

# List volumes
docker volume ls | grep -E 'nexus'

# Remove containers and volumes
docker rm -f nexus
docker volume rm nexus-data
```

### Data Persistence
Nexus use Docker volumes for data persistence

```sh
# List volumes
sudo docker volume ls | grep -E 'nexus'
```

Backup Nexus data volume
```sh
sudo docker run --rm \
  -v nexus-data:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/nexus-backup.tar.gz /data
```
