# Post-Installation Jenkins Configuration Guide

After completing the basic installation, configure essential DevOps tools and integrations for a **production-ready** Jenkins environment.


## Essential Plugins Installation

### Step-1: Access Jenkins Web UI
- Open your browser and navigate to: `http://<YOUR_SERVER_IP>:8080`
- Retrieve the initial **admin** password:
   ```sh
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

- Copy the password and paste it into the Jenkins unlock page
- Follow the setup wizard:
  - **Install suggested plugins** (recommended for beginners)
  - Create your first admin user
  - Configure Jenkins as needed

### Step 2: Access Plugin Manager
1. Navigate to **Manage Jenkins** → **Plugins**
2. Click on **Available plugins** tab
3. Use the search bar to find and install plugins


### Step 3: Install Required Plugins
Install the following plugins and **restart** Jenkins when prompted:

#### 1. Containerization & Orchestration tools
- **Docker Plugin** - Docker integration
- **Docker Pipeline** - Docker commands in pipeline
- **Kubernetes CLI** - Kubectl integration
- **Kubernetes** - Kubernetes deployment

#### 2. Code Quality & Security
- **SonarQube Scanner** - Code quality analysis
- **Trivy** - Container security scanning
- **OWASP Dependency-Check** - Vulnerability detection

#### 3. Build & Artifact Management
- **Maven Integration** - Maven build support
- **Nexus Artifact Uploader** - Nexus Repository integration
- **Artifactory** - JFrog Artifactory support

#### 4. Pipeline & Utilities
- **Pipeline** - Pipeline support (pre-installed)
- **Pipeline Stage View** - Visual pipeline representation
- **Blue Ocean** - Modern Jenkins UI
- **Credentials Binding** - Secure credential management
- **Config File Provider** - Configuration file management

#### 5. Notifications & Collaboration
- **Email Extension** - Advanced email notifications
- **Slack Notification** - Slack integration
- **Microsoft Teams** - Teams notification

#### 6. Install Monitoring Plugins:
- **Monitoring Plugin** - System monitoring
- **Disk Usage Plugin** - Disk space tracking
- **Build History Metrics** - Build statistics
- **Enable Monitoring:**
    - **Manage Jenkins** → **Monitoring** → **Enable**


## Global Tool Configuration
Navigate to **Manage Jenkins** → **Tools** to configure build tools.

### Required Tools
#### 1. JDK (Java Development Kit)
- Name: JDK-21
- JAVA_HOME: `/usr/lib/jvm/java-21-openjdk-amd64`

#### 2. Git
- Name: Default
- Path: git (auto-detected)

#### 3. Maven
- Name: Maven-3.9
- MAVEN_HOME: `/usr/share/maven or auto-install`

#### 4. Gradle (Optional)
- Name: Gradle-8
- Install automatically

#### 5. Docker
- Name: Docker
- Installation root: `/usr/bin`

#### 6. SonarQube Scanner
- Name: Sonar-Scanner
- Install automatically (latest version)

#### 7. Trivy Scanner
- Name: Trivy
- Installation directory: `/usr/local/bin`

#### 8. Node.js (Optional for frontend builds)
- Name: NodeJS-18
- Install automatically

> **Notes:** Verify correct paths and versions
> 
> Mention default tool names in Jenkins for pipelines (`JDK21`, `Maven3.9`, `trivy` etc.)


## Server Configuration
Navigate to **Manage Jenkins** → System to configure **external tool** integrations:

#### 1. SonarQube Server
- Name: `SonarQube`
- Server URL: `http://<YOUR_SERVER_IP>:9000`
- Authentication Token: (Add via Credentials)

#### 2. Nexus Repository Manager
- Server ID: `nexus`
- Server URL: `http://<YOUR_SERVER_IP>:8081`
- Credentials: (Nexus admin credentials)

#### 3. Docker Hub Registry
- Registry URL: `https://hub.docker.com/repositories/<user>`
- Credentials: (Docker credentials)

#### 4. Email Notification (SMTP)
- SMTP Server: `smtp.gmail.com` (or your provider)
- SMTP Port: `465` (SSL) or `587` (TLS)
- Username: Your email
- Password: App password or SMTP password

#### 5. Slack Workspace (Optional)
- Workspace Name: Your workspace
- Credential: Bot OAuth Token
- Default Channel: #jenkins

#### 6. GitHub Server (Optional)
- API URL: `https://github.com/settings/apps`
- Credentials: GitHub Personal Access Token


## Credentials Setup
Navigate to Manage Jenkins → Credentials → (global) → Add Credentials

Credential Type    |  ID                        |  Kind               |  Used For               
-------------------|----------------------------|---------------------|-------------------------
Docker Hub         |  dockerhub-credentials     |  Username/Password  |  Docker image push/pull 
GitHub Token       |  github-token              |  Secret text        |  GitHub API, webhooks     
SSH Deploy Key     |  deployment-ssh-key        |  SSH Private Key    |  Server deployment      
SonarQube Token    |  sonarqube-token           |  Secret text        |  Code quality scanning  
Nexus Credentials  |  nexus-credentials         |  Username/Password  |  Artifact management    
Kubernetes Config  |  kubeconfig                |  Secret file        |  K8s deployments        
AWS Credentials    |  aws-credentials           |  AWS Credentials    |  AWS resource access    
SMTP Email         |  email-smtp                |  Username/Password  |  Email notifications    
Slack Token        |  slack-token               |  Secret text        |  Slack notifications    


## Verification Checklist

After configuration, verify:
- [ ] All plugins installed successfully
- [ ] JDK, Maven, Git tools configured
- [ ] Docker accessible from Jenkins
- [ ] SonarQube server running and connected
- [ ] Nexus repository accessible
- [ ] Email notifications working
- [ ] GitHub webhook configured
- [ ] User permissions set correctly
- [ ] Backup configured
- [ ] Monitoring enabled


---

## Next Steps

Now that your Jenkins server is ready:

1. **Create your first pipeline:**
   - New Item → Pipeline
   - Add Jenkinsfile from Git repository

2. **Configure webhooks:**
   - GitHub → Settings → Webhooks
   - Point to `http://<jenkins-url>:8080/github-webhook/`

3. **Set up Docker registry:**
   - Add Docker Hub credentials
   - Configure image push in pipeline

4. **Implement security scanning:**
   - Add Trivy scan stage in Jenkinsfile
   - Fail builds on critical vulnerabilities

5. **Enable notifications:**
   - Email notifications
   - Slack/Teams integration

---

**Setup Complete!** 🎉
Your Jenkins CI/CD server is now ready for building, testing, and deploying applications with Docker and security scanning capabilities.
