pipeline {
    agent any
    environment {
        DOCKER_USER = 'saquib2006'
        IMAGE_NAME = 'smart-agri-app'
    }
    stages {
        stage('1. Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/saquib-18/Assessment-10.git'
            }
        }
        stage('1.5. Verify Workspace Files') {
            steps {
                bat "dir"
            }
        }
        stage('2. Build Docker Image') {
            steps {
                bat "docker build -t ${DOCKER_USER}/${IMAGE_NAME}:latest ."
            }
        }
        stage('3. Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    bat "echo %PASS% | docker login -u %USER% --password-stdin"
                    bat "docker push ${DOCKER_USER}/${IMAGE_NAME}:latest"
                }
            }
        }
        stage('4. Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'k8s-credentials', variable: 'KUBECONFIG')]) {
                    bat "kubectl apply -f deployment.yaml --kubeconfig=%KUBECONFIG%"
                }
            }
        }
    }
}