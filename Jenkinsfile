pipeline {
    agent any
    environment {
        DOCKER_HUB_USERNAME = 'bhargavram458' // This can be kept as plain text in the script
    }
    stages {
        stage('Checkout Repos') {
            steps {
                script {
                    // Checkout code for all three repositories using credentials for private repos
                    // Checkout Auth-service
                    dir('Auth-service') {
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: '*/master']], // or the branch you want to use
                            userRemoteConfigs: [[
                                url: 'https://github.com/20BQ1A0458/Auth-service.git',
                                credentialsId: 'github-creds'
                            ]]
                        ])
                    }
                    // Checkout Otp-service
                    dir('Otp-service') {
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: '*/master']],
                            userRemoteConfigs: [[
                                url: 'https://github.com/20BQ1A0458/Otp-service.git',
                                credentialsId: 'github-creds'
                            ]]
                        ])
                    }
                    // Checkout Mail-service
                    dir('Mail-service') {
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: '*/master']],
                            userRemoteConfigs: [[
                                url: 'https://github.com/20BQ1A0458/Mail-service.git',
                                credentialsId: 'github-creds'
                            ]]
                        ])
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
                                bat 'docker build -t %DOCKER_HUB_USERNAME%/Auth-service:latest .'
                            }
                        }
                    }
                }
                stage('Build OTP Service') {
                    steps {
                        script {
                            dir('Otp-service') {
                                // Windows command to build Docker image
                                bat 'docker build -t %DOCKER_HUB_USERNAME%/Otp-service:latest .'
                            }
                        }
                    }
                }
                stage('Build Mail Service') {
                    steps {
                        script {
                            dir('Mail-service') {
                                // Windows command to build Docker image
                                bat 'docker build -t %DOCKER_HUB_USERNAME%/Mail-service:latest .'
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
                                bat 'docker push %DOCKER_HUB_USERNAME%/Auth-service:latest'
                            }
                        }
                    }
                }
                stage('Push OTP Service') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-c', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                                // Windows command to push Docker image
                                bat 'docker push %DOCKER_HUB_USERNAME%/Otp-service:latest'
                            }
                        }
                    }
                }
                stage('Push Mail Service') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-c', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                                // Windows command to push Docker image
                                bat 'docker push %DOCKER_HUB_USERNAME%/Mail-service:latest'
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
