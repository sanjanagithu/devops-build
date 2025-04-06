pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('3d9acecb-ef2e-4f9b-97cf-b6ee4583c685') // Replace with your Jenkins credentials ID
        DOCKER_IMAGE = 'sanjanadockerhub/dev'  // Replace with your Docker Hub repo
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'docker-compose build'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                }
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
                script {
                    // Example deployment command (Kubernetes, Docker Swarm, etc.)
                    // Replace with your actual deployment script or commands
                    echo "Deploying to production..."
                    sh './deploy.sh'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker logout'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
