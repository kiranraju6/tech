pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
        DOCKER_IMAGE = "your-dockerhub-username/calculator-app"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/your-username/calculator-app.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                    docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push('latest')
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
