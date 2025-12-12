pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Code Analysis with SonarQube') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn clean verify sonar:sonar'
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t demo-app:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-creds', url: 'https://index.docker.io/v1/']) {
                    sh 'docker tag demo-app:latest niklearn1414/demo-app:latest'
                    sh 'docker push niklearn1414/demo-app:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}