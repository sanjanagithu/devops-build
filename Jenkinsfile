pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('3d9acecb-ef2e-4f9b-97cf-b6ee4583c685') // Add in Jenkins credentials
        IMAGE_NAME = "sanjanadockerhub/dev"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/sanjanagithu/devops-build.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BRANCH_NAME .'
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
                    if (env.BRANCH_NAME == 'dev') {
                        sh "docker tag $IMAGE_NAME:$BRANCH_NAME $IMAGE_NAME-dev:latest"
                        sh "docker push $IMAGE_NAME-dev:latest"
                    } else if (env.BRANCH_NAME == 'master') {
                        sh "docker tag $IMAGE_NAME:$BRANCH_NAME $IMAGE_NAME-prod:latest"
                        sh "docker push $IMAGE_NAME-prod:latest"
                    }
                }
            }
        }
    }
}
