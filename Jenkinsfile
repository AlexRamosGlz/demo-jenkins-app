pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    stages {
        stage('Build') {
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
            steps {
                sh '''
                    echo "Testing..."

                    test -f build/index.html || exit 1

                    npm run test
                '''
            }
        }

        stage('E2E Testing') {
            steps {
                sh '''
                    echo "E2E Testing..."

                    npm install serve

                    node_modules/serve -s build
                    
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
