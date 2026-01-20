pipeline {
    agent any

    stages {

        /* =====================
           BUILD STAGE
        ====================== */
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "=== Build Stage ==="
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        /* =====================
           TEST STAGE
        ====================== */
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "=== Test Stage ==="
                    test -f build/index.html
                    npm test
                '''
            }
        }

        /* =====================
           DEPLOY STAGE (NETLIFY)
        ====================== */
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                NETLIFY_AUTH_TOKEN = credentials('nfp_tAaMoidfLsHEAXQ3RJCeCdQJFTa3xXVN72bc')
                NETLIFY_SITE_ID    = credentials('5e2ae1b4-1177-4829-9ae2-5222dc9091c3')
            }
            steps {
                sh '''
                    echo "=== Deploy Stage ==="
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify deploy --prod --dir=build
                '''
            }
        }
    }
}

