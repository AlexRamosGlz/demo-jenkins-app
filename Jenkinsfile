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
            ls -la
            node --version
            npm --version

            npm ci
            npm run build

            ls -la
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
                    echo "Testing..."

                    test -f build/index.html || exit 1

                    npm run test
                '''
            }
        }

        stage('E2E Testing') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.51.0-noble'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "E2E Testing..."

                    npm install serve

                    node_modules/serve -s build
                    sleep 10
                    
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'            
        }
    }
}
