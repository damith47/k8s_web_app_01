pipeline {
    agent any

    environment {
        IMAGE_NAME = 'lorexhub/lorexweb1'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image ${IMAGE_NAME}:${IMAGE_TAG}"
                bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Docker Login') {
            steps {
                echo 'Logging in to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat """
                       echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image ${IMAGE_NAME}:${IMAGE_TAG}"
                bat "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Updating Kubernetes deployment...'
                bat """
                   kubectl set image deployment/your-deployment-name your-container-name=${IMAGE_NAME}:${IMAGE_TAG} -n your-namespace
                """
            }
        }
    }

    post {
        success {
            echo "Docker image pushed and deployment updated successfully!"
        }
        failure {
            echo "Build, push or deploy failed!"
        }
    }
}
