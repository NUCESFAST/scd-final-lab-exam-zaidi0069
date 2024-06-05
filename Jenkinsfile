pipeline {
    agent any

     environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git 'https://github.com/NUCESFAST/scd-final-lab-exam-zaidi0069.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies for each microservice
                script {
                    dir('auth') {
                        sh 'npm install'
                    }
                    dir('classrooms') {
                        sh 'npm install'
                    }
                    dir('event-bus') {
                        sh 'npm install'
                    }
                    dir('posts') {
                        sh 'npm install'
                    }
                    dir('client') {
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                // Build Docker images for each microservice
                script {
                    def dockerImageTag = "${env.BUILD_NUMBER}" // Use build number as image tag
                    
                    dir('auth') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/auth:${dockerImageTag} ."
                    }
                    dir('classrooms') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/classrooms:${dockerImageTag} ."
                    }
                    dir('event-bus') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/event-bus:${dockerImageTag} ."
                    }
                    dir('posts') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/posts:${dockerImageTag} ."
                    }
                    dir('client') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/client:${dockerImageTag} ./client"
                    }
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                // Push Docker images to Docker Hub
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        dir('auth') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/auth:${dockerImageTag}"
                        }
                        dir('classrooms') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/classrooms:${dockerImageTag}"
                        }
                        dir('event-bus') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/event-bus:${dockerImageTag}"
                        }
                        dir('posts') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/posts:${dockerImageTag}"
                        }
                        dir('client') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/client:${dockerImageTag}"
                        }
                    }
                }
            }
        }
    }
}
