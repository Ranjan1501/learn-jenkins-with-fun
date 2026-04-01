pipeline {
    agent any
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
                    npm install netlify-cli
                    echo "Checking Netlify CLI version"
                    netify --version
                    echo "Deploying to Netlify"
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

