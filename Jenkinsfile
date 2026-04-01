pipeline {
    agent any
    environment {
        NODE_ENV = 'production'
        NETLIFY_PROJECT_ID = '3ee0dbf2-82d6-494f-b190-f3ec67f63c75'
    }
    stages{
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-u root'
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
            agent {
                    docker {
                        image 'node:18-alpine'
                        args '-u root'
                        reuseNode true
                    }
                }
            steps {
                
               sh '''
                    echo "Running tests with pipeline"
                    test -f build/index.html
                    npm test
                    echo "Tests completed successfully"
                '''
            }
        }
        stage ("Deploy") {
            agent {
                    docker {
                        image 'node:18-alpine'
                        args '-u root'
                        reuseNode true
                    }
                }
            steps {
                sh '''
                    echo "Deploying the application"
                    npm install -g netlify-cli
                    echo "Checking Netlify CLI version"
                    netlify
                    echo "Deploying to Netlify"
                    echo "Deploy to : $NODE_ENV"
                    echo "NETLIFY_PROJECT_ID: $NETLIFY_PROJECT_ID"
                '''
            }
        }

    }

    post {
        always {
            echo "Collecting test results"
            junit 'test-results/junit.xml'
        }
    }
}

