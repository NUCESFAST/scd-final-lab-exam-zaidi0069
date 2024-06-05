pipeline {
    agent any

     environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
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
                    dir('Auth') {
                        sh 'npm install'
                    }
                    dir('Classrooms') {
                        sh 'npm install'
                    }
                    dir('event-bus') {
                        sh 'npm install'
                    }
                    dir('Post') {
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
                    
                    dir('Auth') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/Auth:${dockerImageTag} ."
                    }
                    dir('Classroom') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/Classrooms:${dockerImageTag} ."
                    }
                    dir('event-bus') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/event-bus:${dockerImageTag} ."
                    }
                    dir('Post') {
                        sh "docker build -t ${DOCKER_HUB_USERNAME}/Post:${dockerImageTag} ."
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
                        dir('Auth') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/Auth:${dockerImageTag}"
                        }
                        dir('Classroom') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/Classrooms:${dockerImageTag}"
                        }
                        dir('event-bus') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/event-bus:${dockerImageTag}"
                        }
                        dir('Post') {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            sh "docker push ${DOCKER_HUB_USERNAME}/Post:${dockerImageTag}"
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
