pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "Listing workspace"
                    ls -la

                    echo "Checking Node & NPM"
                    node --version
                    npm --version

                    echo "Installing dependencies"
                    npm ci

                    echo "Building app"
                    npm run build

                    echo "Build output"
                    ls -la
                '''
            }
        }
    }
}
