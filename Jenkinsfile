pipeline {
    agent any

    stages {
        stage('Without Docker') {
            steps {
                sh 'echo "Without docker"'
            }
        }
        stage('With Docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                sh 'echo "With docker"'
                sh 'npm --version'
            }
        }
    }
}