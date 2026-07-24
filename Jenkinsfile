pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-app"
        CONTAINER_NAME = "nodejs-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('List Files') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Node Version') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nodejs-app:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop nodejs-app || true
                docker rm nodejs-app || true

                docker run -d \
                  --name nodejs-app \
                  --restart unless-stopped \
                  -p 3000:3000 \
                  nodejs-app:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Completed Successfully'
        }

        failure {
            echo 'CI/CD Pipeline Failed'
        }
    }
}
