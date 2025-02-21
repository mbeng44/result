pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '205930645143'
        AWS_REGION = 'us-east-1'
        IMAGE_NAME = "voting-app1/result"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Primus-Learning/result.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    def image = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${IMAGE_NAME}"
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${image}"
                    sh "docker build -t ${image}:latest ."
                    sh "docker push ${image}:latest"
                }
            }
        }
    }
}
