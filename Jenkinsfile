pipeline {
    agent any

    stages {
      // This is a comment
      /*
        This is a block comment.
        This can be used to disable an entire stage
      */
        stage('Build') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
            steps {
                sh '''
                  ls -la
                  npm --version
                  node --version
                  npm ci
                  npm run build
                  ls -la
                '''
            }
        }
        stage('Test') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
          steps {
            sh '''
              echo 'Test Stage'
              test -f build/index.html
              npm test
            '''
          }
        }
        stage('E2E Test') {
          agent {
            docker {
              image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
              reuseNode true
            }
          }
          steps {
            sh '''
              echo 'E2E Playwright Test Stage'
              npm install -g serve
              serve -s build
              npx playwright test
            '''
          }
        }
    }
    post {
      always {
        junit 'test-results/junit.xml'
      }
    }
}