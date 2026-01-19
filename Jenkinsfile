pipeline {
    agent any

    stages {        
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root:root'
                }
            }

            steps {
                sh '''
                    ls -la
                    node -v
                    npm -v
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
                echo 'Running tests...'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install -g netlify-cli
                    netlify -version
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
            junit 'test-results/junit.xml'
        }
    }
}
