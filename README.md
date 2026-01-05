# CI Pipeline with Jenkins, SonarQube, Docker, and GitHub

## 📌 Project Overview
This project demonstrates a fully automated and configurable **Continuous Integration (CI) pipeline** using **Jenkins**, **GitHub**, **SonarQube**, and **Docker**.  
The pipeline pulls source code from GitHub, performs static code analysis using SonarQube with Quality Gate enforcement, builds a Docker image on successful quality checks, and sends email notifications based on build status.

---

## 🧩 Problem Statement
Set up Jenkins on a local machine and configure a CI pipeline to:
- Pull source code from GitHub
- Perform SonarQube code analysis
- Enforce Quality Gates
- Build Docker image only on successful quality check
- Send email notifications for build success, failure, and aborted states
- Ensure all configurations are externally configurable and not hardcoded

---

## 🎯 Expected Outcome
A fully automated and configurable CI pipeline that:
- Enforces code quality using SonarQube
- Builds Docker images conditionally
- Sends automated email notifications
- Follows DevOps best practices

---

## 🛠 Tools & Technologies Used
- **OS:** Ubuntu (EC2 Instance)
- **CI Tool:** Jenkins
- **Source Control:** GitHub
- **Code Quality:** SonarQube
- **Containerization:** Docker
- **Language:** Python (Sample App)
- **Notifications:** Jenkins Email Extension Plugin

---

## 🏗 High-Level Architecture

Developer
   |
   v
GitHub Repository
   |
   v
Jenkins CI Pipeline
   |
   +--> SonarQube Code Analysis
   |        |
   |        v
   |   Quality Gate Check
   |
   +--> Docker Image Build (Only if Quality Gate PASSES)
   |
   v
Email Notification (Success / Failure / Aborted)

---

## 📂 Project Directory Structure
ci-project/
│
├── Jenkinsfile
│   └── Declarative Jenkins pipeline for CI automation
│
├── Dockerfile
│   └── Defines Docker image build steps for the application
│
├── sonar-project.properties
│   └── SonarQube project configuration file
│
├── requirements.txt
│   └── Python application dependencies
│
├── src/
│   └── app.py
│       └── Sample Python application source code
│
└── README.md
    └── Project documentation and usage details
---

## ⚙️ Jenkins Setup
- Jenkins installed on Ubuntu
- Required plugins installed:
  - Git Plugin
  - Pipeline Plugin
  - SonarQube Scanner Plugin
  - Docker Pipeline Plugin
  - Email Extension Plugin

---

## 🔐 GitHub Integration
- GitHub repository integrated using **Jenkins Credentials Manager**
- No credentials hardcoded in pipeline
- Repository accessed securely using credential ID

---

## 📊 SonarQube Configuration
- SonarQube deployed using Docker
- SonarQube server configured in Jenkins Global Settings
- Authentication token stored in Jenkins Credentials
- **Quality Gate thresholds configured in SonarQube UI (externalized)**

### 🔔 Webhook Configuration
- Webhook configured in SonarQube:

http://<JENKINS-IP>:8080/sonarqube-webhook/

- Enables Jenkins to receive Quality Gate status

---

## 🐳 Docker Configuration
- Dockerfile used to containerize the application
- Docker image built **only if Quality Gate passes**
- Image tagged using Jenkins `BUILD_NUMBER`

---

## 🧪 Jenkins Declarative Pipeline
### Pipeline Stages:
1. Checkout Source Code
2. SonarQube Analysis
3. Quality Gate Validation
4. Docker Image Build
5. Email Notification

### 🔧 Externalized Configuration
All values are configurable outside the Jenkinsfile:
- GitHub repository URL → Jenkins SCM config
- SonarQube project key → Jenkins parameter
- Docker image name → Jenkins parameter
- Email recipients → Jenkins parameter
- Credentials & tokens → Jenkins Credentials Manager
- Quality thresholds → SonarQube Quality Gates

---

## 📩 Email Notifications
- Email sent on:
  - Build SUCCESS
  - Build FAILURE
  - Build ABORTED
- Implemented using Jenkins Email Extension Plugin

---

## 📸 Final Output
The following outputs are generated after successful pipeline execution:
- Jenkins build status: **SUCCESS**
- SonarQube Quality Gate: **PASSED**
- Docker image created successfully
- Email notification received

---

## 🔒 Security & Best Practices
- No hardcoded secrets
- Credentials managed securely in Jenkins
- Quality thresholds managed externally
- Clean and reusable declarative pipeline

---

## ✅ Conclusion
This project successfully implements a production-ready CI pipeline following industry-standard DevOps practices.  
It ensures secure integration, enforces code quality, automates Docker builds, and provides real-time feedback through email notifications.

---

## 👤 Author
**Ashutosh Chandurkar**  
📧 Email: ashutoshmax04@gmail.com  

🔗 GitHub Repository:  
https://github.com/ashutoshmax4/ci-project
