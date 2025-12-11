pipeline {
    agent any
    
    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sinansabithcloud/demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sinancloud/webapplication:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo "$PASS" | docker login -u "$USER" --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker push sinancloud/webapplication:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
