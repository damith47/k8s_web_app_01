pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'v1', description: 'Docker Image Tag')
    }

    environment {
        IMAGE_NAME = 'lorexhub/lorexweb1'
        KUBECONFIG = 'C:\\Users\\Damith.Patabandige\\.kube\\config'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${IMAGE_NAME}:${params.IMAGE_TAG} ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push ${IMAGE_NAME}:${params.IMAGE_TAG}"
            }
        }

        stage('Update Deployment YAML') {
            steps {
                script {
                    def yamlFile = 'k8s/deployment.yaml'
                    def updatedYaml = readFile(yamlFile).replace('__REPLACE_TAG__', params.IMAGE_TAG)
                    writeFile file: yamlFile, text: updatedYaml
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f k8s/deployment.yaml -n lorexapp"
                bat "kubectl rollout status deployment/lorex-app-fe -n lorexapp"
            }
        }
    }

    post {
        success {
            echo "Build and deployment successful!"
        }
        failure {
            echo "Something went wrong."
        }
    }
}
