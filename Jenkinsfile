pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'kshamsuddin/docker-webapp'
    }

    stages {
        stage('Set Version') {
            steps {
                script {
                    env.VERSION = "v1.${BUILD_NUMBER}"
                    echo "Using version: ${env.VERSION}"
                }
            }
        }

        stage('Clone Repository') {
            steps {
                git 'https://github.com/k-shamsuddin/docker-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${env.VERSION}")
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

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                    kubectl set image deployment/webapp webapp=${DOCKER_IMAGE_NAME}:${env.VERSION} --record
                    kubectl rollout status deployment/webapp
                    """
                }
            }
        }

        stage('Clean up Images') {
            steps {
                script {
                    sh "docker rmi ${DOCKER_IMAGE_NAME}:${env.VERSION} || true"
                    sh "docker rmi ${DOCKER_IMAGE_NAME}:latest || true"
                }
            }
        }
    }

    post {
        success {
            echo "Docker images ${DOCKER_IMAGE_NAME}:${env.VERSION} and :latest pushed successfully, deployed to K8s, and cleaned up."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}

