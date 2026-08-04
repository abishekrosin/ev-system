pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/abishekrosin/ev-system.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                    docker compose build
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Stopping existing containers..."
                    docker stop flask_app react_app || true

                    echo "Removing existing containers..."
                    docker rm -f flask_app react_app || true

                    echo "Removing old compose project..."
                    docker compose down || true

                    echo "Starting new containers..."
                    docker compose up -d --build
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    docker ps
                    docker compose ps
                '''
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
            sh '''
                docker ps -a
                docker compose logs || true
            '''
        }

        always {
            cleanWs()
        }
    }
}
