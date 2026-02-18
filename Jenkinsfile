pipeline {
    agent any

    triggers {
        // Automatically check GitHub for changes every minute
        pollSCM('* * * * *')
    }

    environment {
        IMAGE_NAME = "devops-app"
        CONTAINER_NAME = "devops-container"
        APP_PORT = "5000"
    }

    stages {

        stage('Checkout Latest Code') {
            steps {
                echo "Pulling latest code from GitHub"
                git branch: 'main',
                    url: 'https://github.com/<your-username>/devops-practical.git'
            }
        }

        stage('Build Docker Image (No Cache)') {
            steps {
                echo "Building Docker image without cache"
                sh "docker build --no-cache -t ${IMAGE_NAME} ."
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo "Stopping old container if exists"
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
            }
        }

        stage('Run New Container') {
            steps {
                echo "Starting new container"
                sh """
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p ${APP_PORT}:${APP_PORT} \
                ${IMAGE_NAME}
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Application should now be accessible at http://localhost:${APP_PORT}"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful"
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
    }
}
