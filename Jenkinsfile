pipeline {
    agent any

    environment {
        IMAGE_NAME = "lorexhub/lorex-app-fe"
        TAG = "${new Date().format('yyyyMMddHHmmss')}"
        DEPLOYMENT_FILE = "k8s/deployment.yaml"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/damith47/k8s_web_app_01.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Update Deployment YAML') {
            steps {
                sh '''
                sed -i "s|image: .*|image: $IMAGE_NAME:$TAG\\n        imagePullPolicy: Never|" $DEPLOYMENT_FILE
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f $DEPLOYMENT_FILE'
            }
        }

        stage('Rollout Restart (optional)') {
            steps {
                sh 'kubectl rollout restart deployment lorex-app-fe-deployment'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment updated at http://172.20.10.10:30082/"
        }
    }
}
