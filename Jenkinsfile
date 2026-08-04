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
                echo 'Stopping old containers...'
                sh '''
                docker compose down --remove-orphans || true
                docker rm -f flask_app || true
                docker rm -f react_app || true
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                echo 'Building Docker images...'
                sh '''
                export DOCKER_BUILDKIT=0
                docker compose build --no-cache
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying application...'
                sh '''
                docker compose up -d
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking containers...'
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
