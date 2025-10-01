pipeline {
    agent {
        // Use Node 16 Docker image as the build agent
        docker { image 'node:16' }
    }

    // Environment variables for Build process
    environment {
        // REGISTRY = "hub.docker.com" // Registry not needed if Dockerhub is being used
        DOCKER_HUB_USER = "bibeki07"
        IMAGE_NAME = "assignment2"
        TAG = "initial"
    }

    // Install NPM dependencies
    stage('Install NodeJS Dependencies'){
        steps {
            sh 'npm install --save'
        }
    }

    // Run unit test
    stage('Run unit tests'){
        steps {
            sh 'npm test'
        }
    }

    // Run security scan using snyk
    stage('Run security scan'){
        steps {
            sh 'npm install -g snyk@latst'
            sh 'snyk test --severity-threshold=high' // Fail pipeline if any High/Critical issues are detected
        }
    }

    // Build Docker Image
    stage('Build Docker Image') {
        steps {
            sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:$TAG .' 
        }
    }
}
