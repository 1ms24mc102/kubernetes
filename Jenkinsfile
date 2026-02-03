pipeline {
    agent any

    environment {
        IMAGE_NAME = "<dockerhub_username>/flask-ci-cd"
        DOCKERHUB_CRED = credentials('dockerhub')
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                git url: '<github_repo_url>', branch: 'main'
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
                        'dockerhub'
                    ) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
