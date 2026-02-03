pipeline {
    agent any

    environment {
        IMAGE_NAME = "1ms24mc102/kubernetes"
        DOCKERHUB_CRED = credentials('dockerHub')
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                git url: 'https://github.com/1ms24mc102/kubernetes/', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        'dockerHub'
                    ) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
