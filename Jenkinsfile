pipeline {
    agent any

    environment {
        VENV = 'venv'
        DOCKER_IMAGE = 'brohan21/minfy-python-demo'
        DOCKER_CREDENTIALS_ID = 'cf6e9407-6525-4493-beb0-559fb7e052ef'
    }

    stages {
        stage("Install") {
            steps {
                sh '''
                    python3 -m venv $VENV
                    . $VENV/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage("Linting") {
            steps {
                sh '''
                    . $VENV/bin/activate
                    pip install flake8
                    flake8 app/ tests/
                '''
            }
        }

        stage("Testing") {
            steps {
                sh '''
                    . $VENV/bin/activate
                    pip install pytest
                    pytest tests/
                '''
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
