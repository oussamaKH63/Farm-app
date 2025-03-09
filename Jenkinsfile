pipeline {
    agent any
    environment {
        NEXUS_CREDENTIALS = credentials('nexus-credentials')
        NEXUS_URL = '192.168.33.10:8082'
        DOCKER_IMAGE_BACKEND = "${NEXUS_URL}/farm-backend"
        DOCKER_IMAGE_FRONTEND = "${NEXUS_URL}/farm-frontend"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'test', url: 'https://github.com/oussamaKH63/Farm-app.git'
            }
        }
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t $DOCKER_IMAGE_BACKEND:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t $DOCKER_IMAGE_FRONTEND:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Push to Nexus') {
            steps {
                sh 'echo $NEXUS_CREDENTIALS_PSW | docker login -u $NEXUS_CREDENTIALS_USR --password-stdin $NEXUS_URL'
                sh 'docker push $DOCKER_IMAGE_BACKEND:${BUILD_NUMBER}'
                sh 'docker push $DOCKER_IMAGE_FRONTEND:${BUILD_NUMBER}'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
}