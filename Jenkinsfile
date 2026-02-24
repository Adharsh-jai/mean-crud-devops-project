pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "groot321"
        DOCKERHUB_CREDENTIALS = "groot321"
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

        stage('Login to DockerHub Securely') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Images to DockerHub') {
            steps {
                sh 'docker compose push'
            }
        }

        stage('Deploy Latest Images') {
            steps {
                sh """
                cd ${PROJECT_DIR}
                docker compose pull
                docker compose down
                docker compose up -d
                """
            }
        }

        stage('Docker Logout') {
            steps {
                sh 'docker logout'
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
    }
}
