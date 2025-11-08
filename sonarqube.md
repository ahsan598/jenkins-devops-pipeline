# SonarQube Setup with Docker
SonarQube is a code quality and security analysis tool that integrates with Jenkins for continuous inspection.


### Quick Start (Single Container)
Fastest way to get SonarQube running:
```sh
# Update package list
sudo apt update -y

# Install Docker
sudo apt install docker.io -y

# Run SonarQube container
sudo docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:lts-community

# Check container status
sudo docker ps | grep sonarqube

# View logs
sudo docker logs -f sonarqube
```

### Access SonarQube:
- URL: `http://<server_ip>:9000`
- Default credentials: admin / admin
- Change password on first login
