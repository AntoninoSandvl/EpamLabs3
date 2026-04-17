pipeline {
    agent any

    tools {
        nodejs 'node-7.8.0'
    }

    environment {
        IMAGE_NAME = ''
        IMAGE_TAG = 'v1.0'
        HOST_PORT = ''
        CONTAINER_NAME = ''
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Set variables by branch') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        env.IMAGE_NAME = 'nodemain'
                        env.HOST_PORT = '3000'
                        env.CONTAINER_NAME = 'nodemain-container'
                    } else if (env.BRANCH_NAME == 'dev') {
                        env.IMAGE_NAME = 'nodedev'
                        env.HOST_PORT = '3001'
                        env.CONTAINER_NAME = 'nodedev-container'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test -- --watchAll=false'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${env.IMAGE_NAME}:${env.IMAGE_TAG} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker stop ${env.CONTAINER_NAME} || true
                    docker rm ${env.CONTAINER_NAME} || true
                    docker run -d --name ${env.CONTAINER_NAME} -p ${env.HOST_PORT}:3000 ${env.IMAGE_NAME}:${env.IMAGE_TAG}
                """
            }
        }
    }
}
