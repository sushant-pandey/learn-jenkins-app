pipeline {
    agent any

    environment {
      NETLIFY_SITE_ID = '88e016e2-d3f1-48bc-a2fc-057f484d7ee2'
    }

    stages {
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
        stage('Deploy') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
            steps {
                sh '''
                  npm install netlify-cli
                  node_modules/.bin/netlify --version
                  echo "Deploying to Netlify Site Id $NETLIFY_SITE_ID"
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