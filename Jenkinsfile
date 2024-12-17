pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-php-app"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sazaan/my-php-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                docker ps -a | grep php-app && docker stop php-app && docker rm php-app || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh "docker run -d -p 8080:80 --name php-app ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }

        stage('Clean Up Old Images') {
            steps {
                sh "docker image prune -f"
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
    }
}

