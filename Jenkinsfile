pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'kshamsuddin/docker-webapp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/k-shamsuddin/docker-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Image ${DOCKER_IMAGE} pushed."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
