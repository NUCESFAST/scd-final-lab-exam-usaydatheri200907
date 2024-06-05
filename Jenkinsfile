pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('jenkins_token') // Replace with your Docker Hub credentials ID
        DOCKERHUB_REPO = 'usaydather/0907_myapp' // Replace with your Docker Hub repository
        GITHUB_PAT = credentials('github-pat') // Replace with your GitHub PAT credentials ID
        DOCKER_BINARY = '"C:/Program Files/Docker/Docker/resources/bin/docker.exe"' // Specify the full path to Docker executable with double quotes and double backslashes
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/NUCESFAST/scd-final-lab-exam-usaydatheri200907' // Replace with your GitHub repository URL
            }
        }
        
        stage('Build Docker Images') {
            parallel {
                stage('Build Frontend Image') {
                    steps {
                        script {
                            def frontendImage = sh(script: "${env.DOCKER_BINARY} build -t ${env.DOCKERHUB_REPO}:frontend-${env.BUILD_ID} client", returnStdout: true).trim()
                            sh "${env.DOCKER_BINARY} push ${frontendImage}"
                        }
                    }
                }
                stage('Build Auth Image') {
                    steps {
                        script {
                            def authImage = sh(script: "${env.DOCKER_BINARY} build -t ${env.DOCKERHUB_REPO}:auth-${env.BUILD_ID} auth", returnStdout: true).trim()
                            sh "${env.DOCKER_BINARY} push ${authImage}"
                        }
                    }
                }
                stage('Build Classrooms Image') {
                    steps {
                        script {
                            def classroomsImage = sh(script: "${env.DOCKER_BINARY} build -t ${env.DOCKERHUB_REPO}:classrooms-${env.BUILD_ID} classrooms", returnStdout: true).trim()
                            sh "${env.DOCKER_BINARY} push ${classroomsImage}"
                        }
                    }
                }
                stage('Build Event-Bus Image') {
                    steps {
                        script {
                            def eventBusImage = sh(script: "${env.DOCKER_BINARY} build -t ${env.DOCKERHUB_REPO}:event-bus-${env.BUILD_ID} event-bus", returnStdout: true).trim()
                            sh "${env.DOCKER_BINARY} push ${eventBusImage}"
                        }
                    }
                }
                stage('Build Posts Image') {
                    steps {
                        script {
                            def postsImage = sh(script: "${env.DOCKER_BINARY} build -t ${env.DOCKERHUB_REPO}:posts-${env.BUILD_ID} posts", returnStdout: true).trim()
                            sh "${env.DOCKER_BINARY} push ${postsImage}"
                        }
                    }
                }
            }
        }

        stage('Deploy Containers') {
            steps {
                script {
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Test') {
            steps {
                // Add tests to validate your application
                sh 'curl -f http://localhost:907 || exit 1'
            }
        }
    }
}
