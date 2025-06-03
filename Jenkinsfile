pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-app-image'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    runCommand('mvn clean package')
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    runCommand('mvn test')
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    runCommand("docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .")
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    runCommand("docker push ${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

// Helper function to handle OS differences
def runCommand(String command) {
    if (isUnix()) {
        sh command
    } else {
        bat command
    }
}
