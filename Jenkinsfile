pipeline {
    agent any

    tools {
        nodejs 'node-7.8.0'
    }

    environment {
        IMAGE_TAG = 'v1.0'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
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
                script {
                    def imageName = ''
                    if (env.BRANCH_NAME == 'main') {
                        imageName = 'nodemain'
                    } else if (env.BRANCH_NAME == 'dev') {
                        imageName = 'nodedev'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    sh "docker build -t ${imageName}:${env.IMAGE_TAG} ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def imageName = ''
                    def hostPort = ''
                    def containerName = ''

                    if (env.BRANCH_NAME == 'main') {
                        imageName = 'nodemain'
                        hostPort = '3000'
                        containerName = 'nodemain-container'
                    } else if (env.BRANCH_NAME == 'dev') {
                        imageName = 'nodedev'
                        hostPort = '3001'
                        containerName = 'nodedev-container'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    sh """
                        docker stop ${containerName} || true
                        docker rm ${containerName} || true
                        docker run -d --name ${containerName} -p ${hostPort}:3000 ${imageName}:${env.IMAGE_TAG}
                    """
                }
            }
        }
    }
}
