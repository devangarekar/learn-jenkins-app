pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
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

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
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

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
               withCredentials([
                 string(credentialsId: 'netlifytoken', variable: 'NETLIFY_AUTH_TOKEN'),
                 string(credentialsId: 'netlifysite', variable: 'NETLIFY_SITE_ID')
                ])

                ]) {
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
}

