pipeline {
    agent any

    stages {
        stage('Without Docker') {
            steps {
                sh '''
                    echo "Without docker"
                    ls -la
                    touch container-no-docker.txt
                '''
            }
        }
        stage('With Docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    echo "With docker"
                    npm --version
                    touch container-with-docker.txt
                '''
            }
        }
    }
}