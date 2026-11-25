pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'tastoussa'
        SERVER_IMAGE   = "${DOCKERHUB_USER}/mern-server"
        CLIENT_IMAGE   = "${DOCKERHUB_USER}/mern-client"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Server') {
            steps {
                sh 'docker build -t $SERVER_IMAGE:latest ./Server'
            }
        }

        stage('Build Client') {
            steps {
                sh 'docker build -t $CLIENT_IMAGE:latest ./Client'
            }
        }

        stage('Push vers Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS')]) {
                    sh 'echo $DH_PASS | docker login -u $DH_USER --password-stdin'
                    sh 'docker push $SERVER_IMAGE:latest'
                    sh 'docker push $CLIENT_IMAGE:latest'
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