pipeline {
    agent any

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

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                withCredentials([
                    string(credentialsId: 'netlifytoken', variable: 'NETLIFY_AUTH_TOKEN'),
                    string(credentialsId: 'netlifysite', variable: 'NETLIFY_SITE_ID')
                ]) {
                    sh '''
                        echo "=== Deploy Stage ==="
                        npx netlify deploy \
                          --prod \
                          --dir=build \
                          --site=$NETLIFY_SITE_ID \
                          --auth=$NETLIFY_AUTH_TOKEN
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}