pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    environment {
        NETLIFY_SITE_ID = 'd7355b52-ad99-495b-8765-c0c2fd22225c'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            steps {
                sh ''' 
                    ls -la
                    node --version
                    npm --version
                    echo "Building the application..."
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }

            post {
                always {
                    junit 'test-results/junit.xml'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh ''' 
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod --build=false
                '''
            }
        }
        
    }
}
