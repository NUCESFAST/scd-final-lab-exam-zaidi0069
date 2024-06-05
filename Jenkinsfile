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
                        bat 'npm install'
                    }
                    dir('Classrooms') {
                        bat 'npm install'
                    }
                    dir('event-bus') {
                        bat 'npm install'
                    }
                    dir('Post') {
                        bat 'npm install'
                    }
                    dir('client') {
                        bat 'npm install'
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
                        bat "docker build -t ${DOCKER_HUB_USERNAME}/Auth:${dockerImageTag} ."
                    }
                    dir('Classroom') {
                        bat "docker build -t ${DOCKER_HUB_USERNAME}/Classrooms:${dockerImageTag} ."
                    }
                    dir('event-bus') {
                        bat "docker build -t ${DOCKER_HUB_USERNAME}/event-bus:${dockerImageTag} ."
                    }
                    dir('Post') {
                        bat "docker build -t ${DOCKER_HUB_USERNAME}/Post:${dockerImageTag} ."
                    }
                    dir('client') {
                        bat "docker build -t ${DOCKER_HUB_USERNAME}/client:${dockerImageTag} ./client"
                    }
                }
            }
        }

        stage('Pubat Docker Images') {
            steps {
                // Pubat Docker images to Docker Hub
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        dir('Auth') {
                            bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            bat "docker pubat ${DOCKER_HUB_USERNAME}/Auth:${dockerImageTag}"
                        }
                        dir('Classroom') {
                            bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            bat "docker pubat ${DOCKER_HUB_USERNAME}/Classrooms:${dockerImageTag}"
                        }
                        dir('event-bus') {
                            bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            bat "docker pubat ${DOCKER_HUB_USERNAME}/event-bus:${dockerImageTag}"
                        }
                        dir('Post') {
                            bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            bat "docker pubat ${DOCKER_HUB_USERNAME}/Post:${dockerImageTag}"
                        }
                        dir('client') {
                            bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                            bat "docker pubat ${DOCKER_HUB_USERNAME}/client:${dockerImageTag}"
                        }
                    }
                }
            }
        }
    }
}
