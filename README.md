# 🐳 Dockerized Flask Web App

This is a simple Flask web application deployed using Docker. It's a beginner-level DevOps project to understand how containerization works using Docker.

## 📌 What It Does

- A basic Python Flask app with one route (`/`)
- Runs inside a Docker container
- Returns a simple "Hello" message

## 🛠️ Technologies Used

- Python 3.9
- Flask
- Docker

## 📂 Project Structure

docker-webapp/ │ ├── app.py ├── requirements.txt └── Dockerfile

## 🚀 How to Run This Project

### 1. Clone the Repo
```bash
git clone https://github.com/yourusername/docker-webapp.git
cd docker-webapp
```
2. Build the Docker Image
```bash
docker build -t flask-docker-app .
```
3. Run the Docker Container
``` bash
docker run -p 5000:5000 flask-docker-app
```
4. View in Browser
Open: http://localhost:5000
You should see: "Hello, this is a Dockerized Flask App! by Shams."

### 🐳 Docker Image
Available on Docker Hub:  
👉 https://hub.docker.com/r/kshamsuddin/docker-webapp


## 🔁 CI/CD Pipeline Flow

> Fully automated CI/CD with dynamic versioning and latest tag.

1. Jenkins pulls the source code from GitHub.
2. Jenkins sets a dynamic Docker image version using the Git commit hash.
3. Jenkins builds and tags two Docker images:
   - `kshamsuddin/docker-webapp:<commit-hash>`
   - `kshamsuddin/docker-webapp:latest`
4. Both images are pushed to DockerHub.
5. Then Docker images are cleaned up after a successful push.

## 🛠️ Jenkinsfile (Pipeline Script)

``` groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'kshamsuddin/docker-webapp'
        VERSION = '' // Will be set in the pipeline
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/k-shamsuddin/docker-webapp.git'
                script {
                    // You can use a tag, commit hash, or timestamp
                    VERSION = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${VERSION}")
                    latestImage = docker.build("${DOCKER_IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        dockerImage.push()
                        latestImage.push()
                    }
                }
            }
        }

        stage('Clean up Images') {
            steps {
                script {
                    sh "docker rmi ${DOCKER_IMAGE_NAME}:${VERSION} || true"
                    sh "docker rmi ${DOCKER_IMAGE_NAME}:latest || true"
                }
            }
        }
    }

    post {
        success {
            echo "Docker images ${DOCKER_IMAGE_NAME}:${VERSION} and :latest pushed successfully and cleaned up."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
```


👤 Author
Khaja Shamsuddin Ahmed
🔗 [LinkedIn](https://www.linkedin.com/in/khaja-shamsuddin-ahmed)  
💻 [GitHub](https://github.com/k-shamsuddin)
