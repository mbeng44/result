pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'mbeng44'
        IMAGE_NAME = 'mbeng44/result'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS = '8461b105-3d7c-4d95-b34d-da4af62b3a16'
        KUBERNETES_NAMESPACE = 'mbeng'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Primus-Learning/result.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${mbeng44}/${result}:${latest}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 8461b105-3d7c-4d95-b34d-da4af62b3a16) {
                        docker.image("${mbeng44}/${result}:${latest}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                    kubectl apply -f kubernetes/deployment.yaml --namespace=${mbeng}
                    kubectl apply -f kubernetes/service.yaml --namespace=${mbeng}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

