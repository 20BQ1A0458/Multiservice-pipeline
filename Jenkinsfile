pipeline {
    agent any
    environment {
        DOCKER_HUB_USERNAME = 'bhargavram458' // This can be kept as plain text in the script
    }
    stages {
        stage('Checkout Repos') {
            steps {
                script {
                    // Checkout code for all three repositories
                    // Checkout auth-service
                    dir('Auth-service') {
                        checkout scm
                    }
                    // Checkout otp-service
                    dir('Otp-service') {
                        checkout scm
                    }
                    // Checkout mail-service
                    dir('Mail-service') {
                        checkout scm
                    }
                }
            }
        }
        stage('Build Docker Images') {
            parallel {
                stage('Build Auth Service') {
                    steps {
                        script {
                            dir('Auth-service') {
                                // Windows command to build Docker image
                                bat 'docker build -t bhargavram458/auth-service:latest -f "C:\ProgramData\Jenkins\.jenkins\workspace\MultiService Pipeline@2@tmp\multi\AuthService\Dockerfile" "C:\ProgramData\Jenkins\.jenkins\workspace\MultiService Pipeline\Auth-service" '

                            }
                        }
                    }
                }
                stage('Build OTP Service') {
                    steps {
                        script {
                            dir('Otp-service') {
                                // Windows command to build Docker image
                                bat 'docker build -t %DOCKER_HUB_USERNAME%/otp-service:latest .'
                            }
                        }
                    }
                }
                stage('Build Mail Service') {
                    steps {
                        script {
                            dir('Mail-service') {
                                // Windows command to build Docker image
                                bat 'docker build -t %DOCKER_HUB_USERNAME%/mail-service:latest .'
                            }
                        }
                    }
                }
            }
        }
        stage('Push Docker Images') {
            parallel {
                stage('Push Auth Service') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-c', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                                // Windows command to login to Docker Hub using the credentials
                                bat 'docker login -u %DOCKER_HUB_USERNAME% -p %DOCKER_HUB_PASSWORD%'
                                // Windows command to push Docker image
                                bat 'docker push %DOCKER_HUB_USERNAME%/auth-service:latest'
                            }
                        }
                    }
                }
                stage('Push OTP Service') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-c', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                                // Windows command to push Docker image
                                bat 'docker push %DOCKER_HUB_USERNAME%/otp-service:latest'
                            }
                        }
                    }
                }
                stage('Push Mail Service') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-c', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                                // Windows command to push Docker image
                                bat 'docker push %DOCKER_HUB_USERNAME%/mail-service:latest'
                            }
                        }
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Ensure Docker Compose file exists and deploy using it (Windows command)
                    bat 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }
    }
    post {
        always {
            cleanWs() // Clean workspace after run
        }
    }
}
