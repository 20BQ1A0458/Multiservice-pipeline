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
        stage('Build Services') {
            parallel {
                stage('Build Auth Service') {
                    steps {
                        bat 'cd AuthService && docker build -t %DOCKER_REGISTRY%/authservice .'
                    }
                }
                stage('Build OTP Service') {
                    steps {
                        bat 'cd OtpService && docker build -t %DOCKER_REGISTRY%/otpservice .'
                    }
                }
                stage('Build Mail Service') {
                    steps {
                        bat 'cd MailService && docker build -t %DOCKER_REGISTRY%/mailservice .'
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    bat '''
                    REM Navigate to the directory containing docker-compose.yml
                    cd C:\\Users\\gowri\\multi
                    
                    REM Run Docker Compose to bring up the services
                    docker-compose -f docker-compose.yml up -d
                    '''
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
            echo 'Pipeline failed.'
        }
    }
}










