# 🚀 Post-Installation Jenkins Configuration Guide

After installing Jenkins, follow these steps to configure essential **integrations, tools, and credentials** for a production-ready CI/CD environment.


### 🛠️ Install Required Plugins
Navigate to Manage Jenkins → Plugins → Available Plugins and install:

**1. Containerization & Orchestration**
- **Docker Plugin** - Docker integration
- **Docker Pipeline** - Docker commands in pipeline
- **Kubernetes CLI** - Kubectl integration
- **Kubernetes** - Kubernetes deployment

**2. Code Quality & Security**
- **SonarQube Scanner** - Code quality analysis
- **Trivy(or use custom shell wrapper)** - Container security scanning
- **OWASP Dependency-Check** - Vulnerability detection

**3. Build & Artifact Management**
- **Maven Integration** - Maven build support
- **Nexus Artifact Uploader** - Nexus Repository integration

**4. Pipeline & Utilities**
- **Pipeline Stage View** - Visual pipeline representation
- **Blue Ocean** - Modern Jenkins UI
- **Credentials Binding** - Secure credential management
- **Config File Provider** - Configuration file management

**5. Notifications & Collaboration**
- **Email Extension** - Advanced email notifications
- **Slack Notification (optional)** - Slack integration
- **Microsoft Teams (optional)** - Teams notification

**6. Monitoring Plugins:**
- **Monitoring Plugin** - System monitoring
- **Disk Usage Plugin** - Disk space tracking
- **Build History Metrics** - Build statistics
- **Enable Monitoring:**
    - **Manage Jenkins** → **Monitoring** → **Enable**



### 🔧 Global Tool Configuration
Navigate to **Manage Jenkins** → **Tools** to configure build tools.

**1. JDK (Java Development Kit)**
- Name: jdk21
- Path: `/usr/lib/jvm/java-21-openjdk-amd64`

**2. Git**
- Name: git
- Path: auto-detected

**3. Maven**
- Name: maven3
- Path: `/usr/share/maven`

**4. Docker**
- Name: docker
- Path: `/usr/bin`

**5. SonarQube Scanner**
- Name: sonar-scanner
- Path: auto-install

**6. Trivy Scanner**
- Name: trivy
- Path: `/usr/local/bin`



### ⚙️ Configure External Servers
Navigate to **Manage Jenkins** → System to configure **external tool** integrations:

**1. SonarQube Server**
- Name: `sonar`
- Server URL: `http://<YOUR_SERVER_IP>:9000`
- Token: Add via **Credentials → Secret Text**

**2. Nexus Repo**
- Server ID: `nexus`
- Server URL: `http://<YOUR_SERVER_IP>:8081`
- Credentials: Nexus credentials

**3. DockerHub Registry**
- Registry URL: default
- Credentials: DockerHub credentials

**4. Email (SMTP)**
- Server: `smtp.gmail.com`
- Port: `465` (SSL) or `587` (TLS)
- Username: Your email
- Password: Use App Password if using Gmail

**5. Slack / Teams (optional)**
- Add OAuth/Bot tokens

**6. GitHub Integration**
- Add GitHub Personal Access Token (PAT)



### Credentials Setup
Navigate to Manage Jenkins → Credentials → Global → Add Credentials

| ID                      | Type              | Used For               |
| ----------------------- | ----------------- | ---------------------- |
| `dockerhub-credentials` | Username/Password | Push Docker images     |
| `github-token`          | Secret text       | GitHub API + Webhooks  |
| `sonarqube-token`       | Secret text       | Sonar analysis         |
| `nexus-credentials`     | Username/Password | Artifact upload        |
| `kubeconfig`            | Secret file       | Kubernetes deployments |
| `aws-credentials`       | AWS Credentials   | ECR, S3, EC2           |
| `email-smtp`            | Username/Password | SMTP                   |
| `slack-token`           | Secret text       | Notifications          |

---

### 🚀 Next Steps

**Create your first Pipeline:**
1. Go to **New Item → Pipeline**
2. Add your **Jenkinsfile** from Git
3. Configure GitHub webhook:
   - Point to `http://<jenkins-url>:8080/github-webhook/`
4. Add SonarQube & Trivy stages
5. Add Nexus publish + Docker build/push
6. Deploy to Kubernetes via `kubectl` or Helm

---

🎉 Setup Complete — Jenkins is now fully configured for enterprise-grade CI/CD!


### 📚 References
- [Jenkins Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
