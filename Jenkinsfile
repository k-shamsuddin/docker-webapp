pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'kshamsuddin/docker-webapp'
        VERSION = ''
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/k-shamsuddin/docker-webapp.git'
                script {
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

