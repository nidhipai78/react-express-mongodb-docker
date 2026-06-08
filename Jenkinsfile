pipeline {
agent any

environment {
    FRONTEND_IMAGE = "nidhinpai/react-express-mongodb-docker-frontend"
    BACKEND_IMAGE  = "nidhinpai/react-express-mongodb-docker-backend"
}

stages {

    stage('Checkout') {
        steps {
            checkout scm
        }
    }

    stage('Build Frontend Image') {
        steps {
            bat "docker build --target development -t %FRONTEND_IMAGE%:latest ./frontend"
        }
    }

    stage('Build Backend Image') {
        steps {
            bat "docker build --target development -t %BACKEND_IMAGE%:latest ./backend"
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
        }
    }

    stage('Push Backend Image') {
        steps {
            bat "docker push %BACKEND_IMAGE%:latest"
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
        echo 'Pipeline completed successfully'
    }

    failure {
        echo 'Pipeline failed'
    }
}

}