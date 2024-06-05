pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('jenkins_token') // Replace with your Docker Hub credentials ID
        DOCKERHUB_REPO = 'usaydather/0907_myapp' // Replace with your Docker Hub repository
        GITHUB_PAT = credentials('github-pat') // Replace with your GitHub PAT credentials ID
        DOCKER_BINARY = 'C:/Program Files/Docker/Docker/resources/bin/docker.exe' // Specify the full path to Docker executable
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
                            def frontendImage = docker.build("${env.DOCKERHUB_REPO}:frontend-${env.BUILD_ID}", "client")
                            frontendImage.push()
                        }
                    }
                }
                stage('Build Auth Image') {
                    steps {
                        script {
                            def authImage = docker.build("${env.DOCKERHUB_REPO}:auth-${env.BUILD_ID}", "auth")
                            authImage.push()
                        }
                    }
                }
                stage('Build Classrooms Image') {
                    steps {
                        script {
                            def classroomsImage = docker.build("${env.DOCKERHUB_REPO}:classrooms-${env.BUILD_ID}", "classrooms")
                            classroomsImage.push()
                        }
                    }
                }
                stage('Build Event-Bus Image') {
                    steps {
                        script {
                            def eventBusImage = docker.build("${env.DOCKERHUB_REPO}:event-bus-${env.BUILD_ID}", "event-bus")
                            eventBusImage.push()
                        }
                    }
                }
                stage('Build Posts Image') {
                    steps {
                        script {
                            def postsImage = docker.build("${env.DOCKERHUB_REPO}:posts-${env.BUILD_ID}", "posts")
                            postsImage.push()
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
