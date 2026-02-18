pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    environment {
        IMAGE_NAME = "devops-app"
        CONTAINER_NAME = "devops-container"
        APP_PORT = "5000"
    }

    stages {

        stage('Source Code Ready') {
            steps {
                echo "Code already checked out from GitHub by Jenkins"
            }
        }

        stage('Build Docker Image (No Cache)') {
            steps {
                echo "Building Docker image"
                sh "docker build --no-cache -t ${IMAGE_NAME} ."
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo "Stopping old container (if any)"
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
            }
        }

        stage('Run New Container') {
            steps {
                echo "Starting new container"
                sh """
                docker run -d ^
                --name ${CONTAINE
