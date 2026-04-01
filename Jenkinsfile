pipeline {
    agent any
    stages{
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Jenkins is up and running"
                    ls -la
                    node --version
                    npm --version
                    echo "Installing dependencies and building the application with pipeline use npm ci instead of npm install"
                    rm -rf node_modules
                    npm ci 
                    npm run build
                    echo "Build completed successfully"
                    ls -la
                '''
            }
        }
        stage("Test" ) {
            steps {
                echo "test is running and in progress"
            }
        }

    }
}

