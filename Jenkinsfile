pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') 
        DOCKERHUB_NAMESPACE = 'shayoossi' 
        DOCKER_IMAGE_BACKEND = "${DOCKERHUB_NAMESPACE}/farm-backend"
        DOCKER_IMAGE_FRONTEND = "${DOCKERHUB_NAMESPACE}/farm-frontend"
    }
    triggers {
        githubPush() 
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/oussamaKH63/Farm-app.git'
            }
        }
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'sudo docker build -t $DOCKER_IMAGE_BACKEND:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'sudo docker build -t $DOCKER_IMAGE_FRONTEND:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'sudo docker push $DOCKER_IMAGE_BACKEND:${BUILD_NUMBER}'
                sh 'sudo docker push $DOCKER_IMAGE_FRONTEND:${BUILD_NUMBER}'
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
