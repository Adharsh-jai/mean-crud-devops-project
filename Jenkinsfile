pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "groot321"
        DOCKERHUB_CREDENTIALS = "groot321"
        PROJECT_DIR = "D:\\DD\\crud-dd-task-mean-app\\mean-crud-devops-project"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker compose build'
            }
        }

        stage('Login to DockerHub Securely') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    '''
                }
            }
        }

        stage('Push Images to DockerHub') {
            steps {
                bat 'docker compose push'
            }
        }

        stage('Deploy Latest Images') {
            steps {
                bat """
                    cd /d ${PROJECT_DIR}
                    docker compose pull
                    docker compose down
                    docker compose up -d
                """
            }
        }

        stage('Docker Logout') {
            steps {
                bat 'docker logout'
            }
        }
    }

    post {
        always {
            bat 'docker system prune -f'
        }
    }
}
