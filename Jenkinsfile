pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "yourdockerhub"
        IMAGE_NAME = "restaurant-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "restaurant-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/restaurant-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p 8087:8080 --name $CONTAINER_NAME $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

    }
}
