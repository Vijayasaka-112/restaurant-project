pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/YOUR_GITHUB_USERNAME/restaurant-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t restaurant-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:8080 --name restaurant restaurant-app'
            }
        }

    }
}
