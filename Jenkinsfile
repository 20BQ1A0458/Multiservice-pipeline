pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'bhargavram458'  // Replace with your Docker Hub username
        REGISTRY_CREDENTIALS = 'docker-c'  // Replace with the credentials ID in Jenkins
    }
    stages {
        stage('Checkout Auth Service') {
            steps {
                script {
                    // Checkout AuthService repository
                    git credentialsId: 'github-admin-creds', branch: 'master', url: 'https://github.com/qclairvoyance12/AuthService.git'
                }
            }
        }
        stage('Build and Push Auth Service') {
            steps {
                script {
                    try {
                        // Ensure you're in the right directory after checkout
                        echo 'Building AuthService Docker Image'
                        bat 'echo "Current directory: %cd%"'  // To print the current directory
                        bat "docker build -t ${DOCKER_REGISTRY}/authservice ${WORKSPACE}/AuthService"
                        bat "docker push ${DOCKER_REGISTRY}/authservice"
                    } catch (Exception e) {
                        echo "Auth Service Build or Push failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
        stage('Deploy Auth Service with Docker Compose') {
            steps {
                script {
                    try {
                        echo 'Running Docker Compose to deploy AuthService...'
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
