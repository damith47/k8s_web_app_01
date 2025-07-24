pipeline {
    agent any

    environment {
        IMAGE_NAME = "lorexhub/lorex-webapp"
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch to build from')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    bat 'kubectl apply -f k8s/deployment.yaml --validate=false'
                    bat 'kubectl apply -f k8s/service.yaml --validate=false'
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline failed."
        }
        success {
            echo "✅ Deployment completed successfully. App should be available at: http://172.20.10.10:30080/"
        }
    }
}
