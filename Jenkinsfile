pipeline {
    agent any

    stages {
        stage('Checkout Main Repo') {
            steps {
                checkout scm
            }
        }

        stage('Checkout Auth Service Repo') {
            steps {
                git url: 'https://github.com/qclairvoyance12/AuthService.git', 
                    branch: 'master', 
                    credentialsId: 'github-admin-creds', 
                    changelog: false
            }
        }

        stage('Build and Push Auth Service') {
            steps {
                echo "Building AuthService Docker Image"
                script {
                    def authServiceDir = 'C:/ProgramData/Jenkins/.jenkins/workspace/MultiService Pipeline/AuthService'
                    if (fileExists(authServiceDir)) {
                        bat "docker build -t bhargavram458/authservice ${authServiceDir}"
                    } else {
                        error "AuthService directory not found!"
                    }
                }
            }
        }

        stage('Deploy Auth Service with Docker Compose') {
            steps {
                echo "Running Docker Compose to deploy AuthService..."
                script {
                    bat 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }

        stage('Post Actions') {
            steps {
                echo "Cleaning up..."
                bat 'docker system prune -f'
            }
        }
    }
    post {
        failure {
            echo 'Pipeline failed. Please check the logs for errors.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}
