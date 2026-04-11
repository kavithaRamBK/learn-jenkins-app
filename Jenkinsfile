pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '5b1dcd00-7b50-4378-b5e7-3c61922720d8'
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
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
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
                    npm run build
                    npx netlify --version
                    echo "Deploying to production111. Site ID: $NETLIFY_SITE_ID"
                    npx netlify status
                    npx netlify deploy --dir=build --prod
                '''
            }
        }
    }
}
