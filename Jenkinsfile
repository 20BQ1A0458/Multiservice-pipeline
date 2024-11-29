pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'bhargavram458'  // Replace with your Docker Hub username
        REGISTRY_CREDENTIALS = 'docker-c'  // Replace with the credentials ID in Jenkins
    }
    stages {
        stage('Checkout Repositories') {
            parallel {
                stage('Auth Service') {
                    steps {
                        script {
                            git credentialsId: 'github-admin-creds', branch: 'master', url: 'https://github.com/qclairvoyance12/AuthService.git'
                        }
                    }
                }
                stage('OTP Service') {
                    steps {
                        script {
                            git credentialsId: 'github-admin-creds', branch: 'master', url: 'https://github.com/qclairvoyance12/OtpService.git'
                        }
                    }
                }
                stage('Mail Service') {
                    steps {
                        script {
                            git credentialsId: 'github-admin-creds', branch: 'master', url: 'https://github.com/qclairvoyance12/MailService.git'
                        }
                    }
                }
            }
        }
        stage('Build and Push Services') {
            parallel {
                stage('Build and Push Auth Service') {
                    steps {
                        script {
                            try {
                                bat "cd AuthService && echo 'Building AuthService Docker Image' && docker build -t ${DOCKER_REGISTRY}/authservice ."
                                bat "docker push ${DOCKER_REGISTRY}/authservice"
                            } catch (Exception e) {
                                echo "Auth Service Build or Push failed: ${e.message}"
                                currentBuild.result = 'FAILURE'
                            }
                        }
                    }
                }
                stage('Build and Push OTP Service') {
                    steps {
                        script {
                            try {
                                bat "cd OtpService && echo 'Building OTP Docker Image' && docker build -t ${DOCKER_REGISTRY}/otpservice ."
                                bat "docker push ${DOCKER_REGISTRY}/otpservice"
                            } catch (Exception e) {
                                echo "OTP Service Build or Push failed: ${e.message}"
                                currentBuild.result = 'FAILURE'
                            }
                        }
                    }
                }
                stage('Build and Push Mail Service') {
                    steps {
                        script {
                            try {
                                bat "cd MailService && echo 'Building MailService Docker Image' && docker build -t ${DOCKER_REGISTRY}/mailservice ."
                                bat "docker push ${DOCKER_REGISTRY}/mailservice"
                            } catch (Exception e) {
                                echo "Mail Service Build or Push failed: ${e.message}"
                                currentBuild.result = 'FAILURE'
                            }
                        }
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    try {
                        echo 'Running Docker Compose to deploy services...'
                        bat '''
                        cd C:\\Users\\gowri\\multi
                        docker-compose -f docker-compose.yml up -d
                        '''
                    } catch (Exception e) {
                        echo "Docker Compose Deployment failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            bat 'docker system prune -f'  // Clean up Docker resources
        }
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for errors.'
        }
    }
}

