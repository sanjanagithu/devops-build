pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'docker-compose push'
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Add deployment steps here based on your deployment strategy
            }
        }
    }
}
