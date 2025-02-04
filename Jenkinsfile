pipeline {
    agent any

    environment {
      NETLIFY_SITE_ID = '88e016e2-d3f1-48bc-a2fc-057f484d7ee2'
      NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
                  node_modules/.bin/netlify status
                  node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
        stage('Prod E2E Test') {
          agent {
            docker {
              image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
              reuseNode true
              args '-u root:root'
            }
          }
          environment {
            CI_ENVIRONMENT_URL = 'https://papaya-sable-ccc665.netlify.app'
          }
          steps {
            sh '''
              npx playwright test --reporter=html
            '''
          }
        }
    }
    post {
      always {
        junit 'jest-results/junit.xml'
      }
    }
}