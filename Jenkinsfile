pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('3d9acecb-ef2e-4f9b-97cf-b6ee4583c685') // Add in Jenkins credentials
        IMAGE_NAME = "sanjanadockerhub/dev"
        BRANCH_NAME = 'dev'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:dev .'
            }
        }

        stage('Push to Docker Hub') {
            when {
                anyOf {
                    branch 'dev'
                    branch 'master'
                }
            }
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    if ($BRANCH_NAME == 'dev') {
                        sh "docker tag $IMAGE_NAME:dev $IMAGE_NAME-dev:latest"
                        sh "docker push $IMAGE_NAME-dev:latest"
                    } else if ($BRANCH_NAME == 'master') {
                        sh "docker tag $IMAGE_NAME:prod $IMAGE_NAME-prod:latest"
                        sh "docker push $IMAGE_NAME-prod:latest"
                    }
                }
            }
        }
    }
}
