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
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker ps -aq | xargs -r docker stop || true
                    docker ps -aq | xargs -r docker rm || true
                    docker run -d --name ${CONTAINER_NAME} --expose ${HOST_PORT} -p ${HOST_PORT}:3000 ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
    }
}pipeline {
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
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker ps -aq | xargs -r docker stop || true
                    docker ps -aq | xargs -r docker rm || true
                    docker run -d --name ${CONTAINER_NAME} --expose ${HOST_PORT} -p ${HOST_PORT}:3000 ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
    }
}