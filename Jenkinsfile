pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'brohan21/minfy-python-demo'
        DOCKER_CREDENTIALS_ID = '8381c3d5-d3c3-4ae8-b63b-531205251832'
    }

    stages {
        stage("Install") {
            steps {
             echo "Installing"
            }
        }

        stage("Linting") {
            steps {
               echo "linting step"
            }
        }

        stage("Testing") {
            steps {
           echo "Testing"
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage("Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
