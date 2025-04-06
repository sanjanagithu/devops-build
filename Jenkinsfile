pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('3d9acecb-ef2e-4f9b-97cf-b6ee4583c685')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/sanjanagithu/devops-build.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("sanjanadockerhub/dev:dev")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', '3d9acecb-ef2e-4f9b-97cf-b6ee4583c685') {
                        docker.image("sanjanadockerhub/dev:dev").push()
                    }
                }
            }
        }
    }
}
