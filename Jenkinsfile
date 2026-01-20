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
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        NETLIFY_SITE_ID    = credentials('netlify-site-id')
    }
    steps {
        sh '''
            echo "=== Deploy Stage ==="
            npm install netlify-cli@20.1.1

            node_modules/.bin/netlify deploy \
              --prod \
              --dir=build \
              --site=$NETLIFY_SITE_ID \
              --auth=$NETLIFY_AUTH_TOKEN
        '''
    }
}

    }
}

