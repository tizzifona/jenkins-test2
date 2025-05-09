pipeline {
    agent any

    environment {
        IMAGE_NAME = 'tuusuario/tuimagen'
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub-creds'
        GITHUB_CREDENTIALS_ID = 'jenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/tizzifona/jenkins-test2',
                    credentialsId: "${GITHUB_CREDENTIALS_ID}"
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('DockerHub Login and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:latest
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}
