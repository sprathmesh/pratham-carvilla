
pipeline {
    agent any

    environment {
        COMPOSE_FILE = "docker-compose.yml"
        CONTAINER_NAME = "carvilla-website"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo "Pulling latest changes from GitHub..."
                    checkout scm
                }
            }
        }

        stage('Stop Existing Container') {
            steps {
                script {
                    echo "Stopping and removing any existing container..."
                    sh "docker ps -q --filter name=$CONTAINER_NAME | xargs -r docker stop"
                    sh "docker ps -aq --filter name=$CONTAINER_NAME | xargs -r docker rm"
                }
            }
        }

        stage('Deploy New Version') {
            steps {
                script {
                    echo "Deploying the updated project..."
                    sh "docker-compose down"
                    sh "docker-compose up -d --build"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed! Check logs for details."
        }
    }
}
