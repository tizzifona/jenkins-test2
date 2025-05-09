pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'chmod 666 /var/run/docker.sock || true'

                sh 'docker build --no-cache -t jenkins-test2 .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f jenkins-test2 || true'

                sh 'docker run -d -p 8081:80 --name jenkins-test2 jenkins-test2'
            }
        }
    }
}
