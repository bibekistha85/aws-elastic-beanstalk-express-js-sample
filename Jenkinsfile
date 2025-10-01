pipeline {
    agent { label 'docker' } // Agent with Docker installed
    environment {
        DOCKER_HUB_USER = "bibeki07"
        IMAGE_NAME = "assignment2"
        TAG = "initial"
    }
    stages {
        stage('Install NodeJS Dependencies') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('Run unit tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Run security scan') {
            steps {
                sh 'npm install -g snyk@latest'
                sh 'snyk test --severity-threshold=high'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:$TAG .'
            }
        }
    }
}
