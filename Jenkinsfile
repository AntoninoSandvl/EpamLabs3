pipeline {
    agent any

    environment {
        APP_NAME = "cicd-app"
        PORT = ""
        CONTAINER_NAME = ""
        IMAGE_NAME = ""
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Set variables by branch') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        env.PORT = '3000'
                        env.CONTAINER_NAME = 'cicd-main'
                        env.IMAGE_NAME = 'cicd-main-image'
                    } else if (env.BRANCH_NAME == 'dev') {
                        env.PORT = '3001'
                        env.CONTAINER_NAME = 'cicd-dev'
                        env.IMAGE_NAME = 'cicd-dev-image'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building branch ${env.BRANCH_NAME}"
            }
        }

        stage('Test') {
            steps {
                echo "Running tests for ${env.BRANCH_NAME}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${env.IMAGE_NAME}:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker rm -f ${env.CONTAINER_NAME} || true
                    docker run -d --name ${env.CONTAINER_NAME} -p ${env.PORT}:3000 ${env.IMAGE_NAME}:${env.BRANCH_NAME}
                """
            }
        }
    }
}