pipeline {
agent any

```
environment {
    GIT_REPO        = "https://github.com/YOUR_USERNAME/restaurant-project.git"
    GIT_BRANCH      = "main"

    DOCKERHUB_USER  = "vijaya123qw"
    IMAGE_NAME      = "restaurant-app"
    IMAGE_TAG       = "${BUILD_NUMBER}"

    DOCKER_CREDS    = "dockercred"

    CONTAINER_NAME  = "restaurant-container"
    HOST_PORT       = "9676"
    CONTAINER_PORT  = "80"
}

stages {

    stage('Checkout Code') {
        steps {
            checkout scmGit(
                branches: [[name: '*/main']],
                userRemoteConfigs: [[
                    credentialsId: 'githubcred',
                    url: "${GIT_REPO}"
                ]]
            )
        }
    }

    stage('Build Docker Image') {
        steps {
            sh """
            docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
            """
        }
    }

    stage('DockerHub Login') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: "${DOCKER_CREDS}",
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {

                sh """
                echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                """
            }
        }
    }

    stage('Push Image to DockerHub') {
        steps {
            sh """
            docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
            """
        }
    }

    stage('Deploy Container') {
        steps {
            sh """
            docker stop ${CONTAINER_NAME} || true
            docker rm ${CONTAINER_NAME} || true

            docker run -d \
            -p ${HOST_PORT}:${CONTAINER_PORT} \
            --name ${CONTAINER_NAME} \
            ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
            """
        }
    }

}
```

}
