pipeline {
    agent none // We will define agents per stage
    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
		DOCKER_HUB_PASSWORD = credentials('DOCKER_HUB_PASSWORD') // Docker Hub password stored in jenkins credentials manager
        DOCKER_HUB_USER = "bibeki07"
        IMAGE_NAME = "assignment2"
        TAG = "initial"
    }
    stages {
		// Install NodeJS depenedencies in packages.json using npm install
        stage('Install NodeJS Dependencies') {
            agent {
                docker {
                    image 'node:16'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm install --save'
            }
        }
		
		// Run unit tests
        stage('Run unit tests') {
             agent {
                docker {
                    image 'node:16'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm test || echo "no test found"'
            }
        }

		// Run security scan using snyk - https://snyk.io
        stage('Run security scan') {
             agent {
                docker {
                    image 'node:16'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm install -g snyk@latest' // Install snyk package globally
                sh 'snyk auth $SNYK_TOKEN' // SNYK_TOKEN stored in jenkins credentials
                sh 'snyk test --severity-threshold=high' // Snyk test for high severity/critical, this will fail the pipeline if High/Critical issues are detected
           }
        }
   
        stage('Build Docker Image and Push to Dockerhub') {
             agent {
                docker {
                    image 'docker:24'
                    args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
				// Build, and push docker image to docker hub.
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:$TAG .'
				sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USER --password-stdin'
				sh 'docker push $DOCKER_HUB_USER/$IMAGE_NAME:$TAG'
            }
        }
    }
}
