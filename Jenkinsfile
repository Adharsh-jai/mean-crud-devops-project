pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = "dockerhub-credentials"
        DOCKERHUB_USERNAME = "groot321"
        PROJECT_DIR = "/home/ubuntu/mean-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Images') {
            steps {
                sh 'docker compose push'
            }
        }

        stage('Deploy Application') {
            steps {
                sh """
                cd ${PROJECT_DIR}
                docker compose pull
                docker compose down
                docker compose up -d
                """
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
    }
}
