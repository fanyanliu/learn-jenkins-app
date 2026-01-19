pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '3bd540dd-1fde-47c9-8836-2ff7f8d1f513'
        NETLIFY_AUTH_TOKEN = credentials('Netlify-token')
    }

    stages {
        // stage('Fix permissions') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //             args '-u root'
        //         }
        //     }

        //     steps {
        //         sh '''
        //             echo "Fixing permissions..."
        //             chown -R node:node /var/jenkins_home/workspace/learn-jenkins-app/node_modules/playwright
        //         '''
        //     }
        // }
        // stage('Clean') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //             args '-u root'
        //         }
        //     }

        //     steps {
        //         sh '''
        //             echo "Cleaning workspace..."
        //             rm -rf node_modules
        //         '''
        //     }
        // }

        // stage('Build') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //             args '-u root'
        //         }
        //     }

        //     steps {
        //         sh '''
        //             node -v
        //             npm -v

        //             rm -rf node_modules
        //             npm ci
        //             npm run build
        //         '''
        //     }
        // }

        // stage('Test') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }

        //     steps {
        //         echo 'Running tests...'
        //         sh '''
        //             test -f build/index.html
        //             npm test
        //         '''
        //     }
        // }
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
                    npm install netlify-cli --save-dev
                    
                    echo "Deploying to Netlify site ID: $NETLIFY_SITE_ID"
                    netlify login:auth --auth $NETLIFY_AUTH_TOKEN
                    netlify status
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
