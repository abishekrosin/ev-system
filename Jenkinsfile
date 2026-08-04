pipeline {
<<<<<<< HEAD
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
=======
 agent any
 stages {
 stage('Clone') {
 steps {
 echo 'Cloning Source Code...'
 checkout scm
 }
 }
 stage('Cleanup Old Containers') {
 steps {
 echo 'Removing old containers...'
 sh '''
 docker compose down --remove-orphans || true
 docker rm -f ev-system-backend || true
 docker rm -f ev-system-frontend || true
 docker container prune -f || true
 '''
 }
 }
 stage('Build Docker Images') {
 steps {
 echo 'Building Docker Images...'
 sh 'docker compose build'
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
>>>>>>> b003310 (Updated Jenkinsfile and backend changes)
}
