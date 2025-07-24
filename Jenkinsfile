pipeline {
    agent any

    parameters {
        string(name: 'TAG', defaultValue: '1.0.0', description: 'Docker Image Tag')
    }

    environment {
        IMAGE_NAME = 'lorexhub/lorexweb1'
        IMAGE_TAG = "${params.TAG}"
        KUBECONFIG = 'C:\\Users\\Damith.Patabandige\\.kube\\config'
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
                echo "Updating Kubernetes deployment to use image: ${IMAGE_NAME}:${IMAGE_TAG}"
                bat """
                    kubectl set image deployment/lorex-app-fe-deployment lorex-app-fe=${IMAGE_NAME}:${IMAGE_TAG} -n lorexapp
                    kubectl rollout status deployment/lorex-app-fe-deployment -n lorexapp
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build, push, and deployment completed successfully with tag: ${IMAGE_TAG}"
        }
        failure {
            echo "❌ Build, push, or deploy failed."
        }
    }
}
