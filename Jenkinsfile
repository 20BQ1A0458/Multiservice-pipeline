pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'bhargavram458'  // Replace with your Docker Hub username
        REGISTRY_CREDENTIALS = 'docker-c'  // Replace with the credentials ID for Docker Hub
        GITHUB_CREDENTIALS = 'github-admin-creds'  // Replace with the credentials ID for GitHub
    }
    stages {
        stage('Checkout Repositories') {
            parallel {
                stage('Auth Service') {
                    steps {
                        script {
                            echo 'Cloning AuthService repository'
                            git credentialsId: GITHUB_CREDENTIALS, branch: 'main', url: 'https://github.com/qclairvoyance12/AuthService.git'
                        }
                    }
                }
                stage('OTP Service') {
                    steps {
                        script {
                            echo 'Cloning OtpService repository'
                            git credentialsId: GITHUB_CREDENTIALS, branch: 'main', url: 'https://github.com/qclairvoyance12/OtpService.git'
                        }
                    }
                }
                stage('Mail Service') {
                    steps {
                        script {
                            echo 'Cloning MailService repository'
                            git credentialsId: GITHUB_CREDENTIALS, branch: 'main', url: 'https://github.com/qclairvoyance12/MailService.git'
                        }
                    }
                }
            }
        }
        stage('Build Services') {
            parallel {
                stage('Build Auth Service') {
                    steps {
                        script {
                            echo 'Building AuthService Docker image'
                            bat 'cd AuthService && docker build -t %DOCKER_REGISTRY%/authservice .'
                        }
                    }
                }
                stage('Build OTP Service') {
                    steps {
                        script {
                            echo 'Building OtpService Docker image'
                            bat 'cd OtpService && docker build -t %DOCKER_REGISTRY%/otpservice .'
                        }
                    }
                }
                stage('Build Mail Service') {
                    steps {
                        script {
                            echo 'Building MailService Docker image'
                            bat 'cd MailService && docker build -t %DOCKER_REGISTRY%/mailservice .'
                        }
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    echo 'Deploying with Docker Compose'
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
