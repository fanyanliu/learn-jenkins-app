pipeline {
    agent any

    stages {
        stage('Fix permissions') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }

            steps {
                sh '''
                    echo "Fixing permissions..."
                    chown -R node:node /var/jenkins_home/workspace/learn-jenkins-app/node_modules/playwright
                '''
            }
        }
        stage('Clean') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }

            steps {
                sh '''
                    echo "Cleaning workspace..."
                    rm -rf node_modules
                '''
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }

            steps {
                sh '''
                    node -v
                    npm -v

                    rm -rf node_modules
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
                    args '-u root'
                }
            }

            steps {
                sh '''
                    whoami
                    npm install netlify-cli
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
