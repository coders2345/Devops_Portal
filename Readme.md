# 🚀 Flask Portfolio CI/CD Pipeline Deployment

This repository contains a simple **Python Flask web application** deployed using a complete CI/CD pipeline setup with **Jenkins**, **Docker**, and **Render Cloud**.

---

## 📌 Project Overview

The goal of this project is to demonstrate:

* Automated deployment of a web app using Jenkins CI/CD pipeline.
* Containerization using Docker.
* Deployment to a free-tier cloud platform (Render).
* Secrets and credentials management using Jenkins.

---

## 🛠️ Technologies Used

| Component          | Technology                     |
| ------------------ | ------------------------------ |
| Language           | Python 3.11                    |
| Framework          | Flask 3.1.1                    |
| CI/CD Tool         | Jenkins (Declarative Pipeline) |
| Containerization   | Docker                         |
| Container Registry | DockerHub                      |
| Cloud Hosting      | Render                         |
| Version Control    | Git & GitHub                   |

---

## 🧪 Run the App Locally

```bash
# Clone the repository
git clone https://github.com/coders2345/Devops_Portal.git
cd Devops_Portal

# Create and activate a virtual environment (optional)
python -m venv venv
venv\Scripts\activate  # On Windows

# Install dependencies
pip install -r requirements.txt

# Run the Flask app
python app.py
```

🗽 Open in browser: `http://localhost:5000`

---

## 🐳 Run the App with Docker

```bash
# Build the Docker image
docker build -t flask-portfolio .

# Run the container
docker run -d -p 5000:5000 flask-portfolio
```

📍 Visit: `http://localhost:5000`

---

## ⚙️ CI/CD Pipeline Overview

The Jenkins pipeline performs the following steps automatically:

| Step                | Description                                |
| ------------------- | ------------------------------------------ |
| ✅ Git Checkout      | Pulls latest code from GitHub              |
| ✅ Build Image       | Builds Docker image for Flask app          |
| ✅ Health Check      | Runs a temporary container and checks HTTP |
| ✅ Push to DockerHub | Pushes image to DockerHub                  |
| ✅ Deploy to Render  | Triggers deployment using Render webhook   |

---

## 🌐 Cloud Deployment (Render)

This app is deployed on Render using **Docker-based deployment**.

🔗 **Live URL**: [https://devops-portal-task-automated.onrender.com](https://devops-portal-task-automated.onrender.com)

Render pulls the latest image when triggered by Jenkins via webhook.

---

## 🔐 Secrets Used in Jenkins

| Credential ID        | Description                           |
| -------------------- | ------------------------------------- |
| `dockerhub-creds`    | DockerHub username and password       |
| `RENDER_DEPLOY_HOOK` | Render webhook URL (stored as string) |

These are securely added in: **Jenkins > Manage Jenkins > Credentials**

---

## 🗘️ Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-portfolio"
        CONTAINER_NAME = "flask-portfolio"
        PORT = "5000"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Run Container for Test') {
            steps {
                echo 'Running Docker container (temp for testing)...'
                bat "docker rm -f temp-test || exit 0"
                bat "docker rm -f %CONTAINER_NAME% || exit 0"
                bat "docker run -d -p %PORT%:5000 --name temp-test %IMAGE_NAME%"
                bat "ping 127.0.0.1 -n 6 > nul"
            }
        }

        stage('Test Application') {
            steps {
                echo 'Performing health check...'
                bat 'curl -s http://localhost:5000 | findstr "Portfolio" || exit /b 1'
            }
        }

        stage('Clean Up Test Container') {
            steps {
                echo 'Cleaning up test container...'
                bat "docker rm -f temp-test"
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing image to DockerHub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_PASSWORD'
                )]) {
                    bat """
                        docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%
                        docker tag %IMAGE_NAME% %DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest
                        docker push %DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest
                    """
                }
            }
        }

        stage('Deploy to Render') {
            steps {
                echo 'Triggering deployment to Render...'
                withCredentials([string(credentialsId: 'RENDER_DEPLOY_HOOK', variable: 'RENDER_HOOK')]) {
                    bat "curl -X POST %RENDER_HOOK%"
                }
            }
        }
    }
}
```

---

## ✅ Assignment Checklist

| Requirement                   | Status |
| ----------------------------- | ------ |
| GitHub Repository             | ✅ Done |
| Flask App                     | ✅ Done |
| Dockerfile Created            | ✅ Done |
| Jenkins Pipeline Configured   | ✅ Done |
| DockerHub Push                | ✅ Done |
| Render Deployment             | ✅ Done |
| Secrets Managed in Jenkins    | ✅ Done |
| Health Check in CI            | ✅ Done |
| README with Full Instructions | ✅ Done |

---

## 📬 Author

**Mabasha R**
🎓 B.Tech AI & DS
📧 [mabasham52@gmail.com](mailto:mabasham52@gmail.com)
🐙 GitHub: [@coders2345](https://github.com/coders2345)

---
