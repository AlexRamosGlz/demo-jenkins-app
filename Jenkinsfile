pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23-alpine'
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
                    image 'node:18-alipne'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "Testing...

                    test -f build/index.html || exit 1

                    npm run test
                '''
            }
        }
    }
}
