pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                echo 'Cloning source code from GitHub...'
                git branch: 'master',
                    url: 'https://github.com/abishekrosin/ev-system.git'
            }
        }

        stage('Cleanup Old Containers') {
            steps {
                echo 'Removing old containers...'
                sh '''
                docker compose down --remove-orphans || true
                docker rm -f flask_app || true
                docker rm -f react_app || true
                docker container prune -f || true
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                echo 'Building Docker Images...'
                sh 'docker compose build --no-cache'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Starting Containers...'
                sh 'docker compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking Running Containers...'
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
        }
        always {
            echo 'Pipeline Completed.'
        }
    }
}
