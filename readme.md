# 🚀 Jenkins CI/CD Pipeline Setup

A comprehensive guide for setting up and deploying a **Jenkins-based** CI/CD environment integrated with **Docker** for containerized builds, **SonarQube** for code quality analysis, **Nexus** for artifact management, and **Trivy** for security vulnerability scanning — enabling automated, secure, and reliable software delivery pipelines.


This setup provides a complete Jenkins automation server configured with essential DevOps tools:
- **Jenkins** - CI/CD automation server used to orchestrate builds, tests, and deployments
- **Docker** - Containerization for packaging, building, and running applications across environments
- **SonarQube** - Code quality and static analysis platform (bugs, vulnerabilities, etc..)
- **Nexus** - Artifact repository manager for Maven, npm, Docker images, stores and serves build artifacts
- **Trivy** - Vulnerability scanner for container images, file systems, and IaC configurations.

### 🧠 Pipeline Workflow
```txt
Developer → Git → Jenkins Pipeline → SonarQube Scan → Trivy Scan → Nexus Upload → Docker Build → Kubernetes Deployment
```

### 🎯 Key Objectives

- Set up DevOps infrastructure on AWS EC2
- Automate CI/CD using Jenkins Pipelines
- Perform quality & security scanning
- Store build artifacts in Nexus
- Deploy containerized apps on Kubernetes


### 📂 Project Structure:
```txt
Jenkins-Pipeline-Setup/
├── Jenkins
├── SonarQube
├── Nexus
├── source-code
├── scripts/
└── files/
```


### ⚙️ Setup Overview
| Phase                     | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| **1. Jenkins Setup**      | Install Jenkins, Docker, Trivy & configure pipeline plugins |
| **2. SonarQube & Trivy**  | Code quality + Security scanning                            |
| **3. Docker & Nexus**     | Containerized app + Artifact storage                        |
| **4. Kubernetes Cluster** | Setup & connect Jenkins for deployments                     |
| **5. Pipeline Execution** | Automate end-to-end CI/CD with Jenkinsfile                  |


### 📦 Pipeline Stages
1. Checkout Code
2. Build & Test (Maven / App build tool)
3. SonarQube Static Code Analysis
4. Trivy Security Scan
5. Publish Artifact to Nexus
6. Docker Image Build & Push
7. Deploy to Kubernetes


### 🔍 Tool Access & Ports
| Tool      | Port | URL                     |
| --------- | ---- | ----------------------- |
| Jenkins   | 8080 | http://<server-ip>:8080 |
| SonarQube | 9000 | http://<server-ip>:9000 |
| Nexus     | 8081 | http://<server-ip>:8081 |


### 🏁 Conclusion

This pipeline provides a production-ready DevOps workflow that ensures consistent, secure, and automated application delivery across environments.
