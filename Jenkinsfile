pipeline {
    agent {
        docker {
            image 'node:18'
            reuseNode true
            args '-u root'
        }
    }

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

        stage('Build') {
            steps {
                sh '''
                    node -v
                    npm -v

                    echo "🔧 Cleaning..."
                    rm -rf node_modules

                    echo "📦 Installing dependencies..."
                    npm ci

                    echo "🏗️ Building the project..."
                    npm run build

                    echo "✅ Build complete"
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "🧪 Running tests..."
                    test -f build/index.html
                    npm test
                    echo "✅ Tests passed"
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    echo "🚀 Deploying to Netlify..."
                    npx netlify-cli deploy \
                        --dir=build \
                        --prod \
                        --auth "$NETLIFY_AUTH_TOKEN" \
                        --site "$NETLIFY_SITE_ID"

                    echo "🚀 Deployment complete"
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
