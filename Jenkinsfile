pipeline {
agent any

stages {

    stage('Checkout') {
        steps {
            checkout scm
        }
    }

    stage('Build') {
        steps {
            bat 'docker compose build'
        }
    }

    stage('Run Containers') {
        steps {
            bat 'docker compose up -d'
        }
    }
}

}