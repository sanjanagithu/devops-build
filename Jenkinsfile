pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('3d9acecb-ef2e-4f9b-97cf-b6ee4583c685')
        IMAGE_NAME = "sanjanadockerhub/dev:dev"
        CONTAINER_NAME = "dev_container"
        PORT = "80"
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
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', '3d9acecb-ef2e-4f9b-97cf-b6ee4583c685') {
                        docker.image("${IMAGE_NAME}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                script {
                    sh """
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                        docker pull ${IMAGE_NAME}
                        docker run -d -p ${PORT}:${PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    """
                }
            }
        }
    }
}
