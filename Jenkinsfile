pipeline {
    agent any

    tools {
        maven 'Maven-3.9.8'
        jdk 'JDK-21'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git url: 'https://github.com/sep-2024-trivandrum/authentication-service.git', branch: 'main'
            }
        }

        stage('Pre-Build') {
            steps {
                // Pre-build Docker cleanup steps
                bat '''
                docker stop authentication-service || true
                docker rm authentication-service || true
                docker rmi -f authentication-service:latest || true
                '''
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                bat 'mvn clean install'
            }
        }

        stage('Post-Build') {
            steps {
                // Build and run the Docker container
                bat '''
                docker build -t authentication-service:latest .
                docker run -d -p 9095:9095 --name authentication-service authentication-service:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
