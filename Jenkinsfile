pipeline {
        agent any
    
        tools {
            maven "Maven"
        }
        environment {
            DOCKER_REGISTRY = 'docker.io'
            DOCKER_CREDENTIALS = credentials('dockercred') 
            IMAGE_NAME = 'sawat98/vprofile'
            IMAGE_TAG = "${env.BUILD_NUMBER}"
        }
        stages {
            stage('Clean WorkSpace') {
                steps {
                    cleanWs()
                }
            }
            stage('Checkout') {
                steps {
                    git 'https://github.com/Sarwat98/vprofile-project.git'
                }
            }
            stage('Build') {
                steps {
                    sh "mvn clean -DskipTests"
                }
            }
            stage('Build Docker Image') {
                steps {
                    script {
                        docker.build("${IMAGE_NAME}:${IMAGE_TAG}","-f Maven.Dockerfile .")
                    }
                }
            }
            stage('Push Docker Image') {
                steps {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockercred',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        script {
                            sh '''
                                docker login -u "$DOCKER_USER" -p "$DOCKER_PASS" docker.io
                                docker push "${IMAGE_NAME}:${IMAGE_TAG}"
                                docker logout
                            '''
                        }
                    }    
                }
            }
            
        }
    }
