pipeline {
agent any

environment {
    DOCKER_USER = "nidhinpai"

    FRONTEND_IMAGE = "${DOCKER_USER}/react-express-mongodb-docker-frontend"
    BACKEND_IMAGE  = "${DOCKER_USER}/react-express-mongodb-docker-backend"

    IMAGE_TAG = "${BUILD_NUMBER}"
}

stages {

    stage('Checkout') {
        steps {
            checkout scm
        }
    }

    stage('Build Frontend Image') {
        steps {
            bat "docker build --target development -t %FRONTEND_IMAGE%:latest -t %FRONTEND_IMAGE%:%IMAGE_TAG% ./frontend"
        }
    }

    stage('Build Backend Image') {
        steps {
            bat "docker build --target development -t %BACKEND_IMAGE%:latest -t %BACKEND_IMAGE%:%IMAGE_TAG% ./backend"
        }
    }

    stage('Docker Login') {
        steps {
            withCredentials([
                usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )
            ]) {
                bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
            }
        }
    }

    stage('Push Frontend Image') {
        steps {
            bat "docker push %FRONTEND_IMAGE%:latest"
            bat "docker push %FRONTEND_IMAGE%:%IMAGE_TAG%"
        }
    }

    stage('Push Backend Image') {
        steps {
            bat "docker push %BACKEND_IMAGE%:latest"
            bat "docker push %BACKEND_IMAGE%:%IMAGE_TAG%"
        }
    }

    stage('Deploy') {
        steps {
            bat 'docker compose up -d'
        }
    }
}

post {
    success {
        echo "Pipeline completed successfully."
        echo "Images pushed with tags: latest and ${IMAGE_TAG}"
    }

    failure {
        echo "Pipeline failed."
    }
}

}