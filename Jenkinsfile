pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/itai53/super-app.git'
            }
        }

        stage('Build Docker Images') {
            steps {

                sh 'docker-compose build'
            }
        }

        stage('Test Docker Compose App') {
            steps {

                sh 'docker-compose up -d'
                
                sleep 10

                sh 'curl http://10.0.0.14:3000/super-app'
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {

                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                
                sh '''
                    docker tag super-app_node:latest $DOCKER_USERNAME/super-app-node:latest
                    docker push $DOCKER_USERNAME/super-app-node:latest

                    docker tag super-app_php:latest $DOCKER_USERNAME/super-app-php:latest
                    docker push $DOCKER_USERNAME/super-app-php:latest
                '''
            }
        }
    }

    environment {
        DOCKER_USERNAME = credentials('dockerhub-username')
        DOCKER_PASSWORD = credentials('dockerhub-password')
   }
}
