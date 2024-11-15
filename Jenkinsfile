pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = "5779b6de-37ec-4e6b-b509-c35ae655f3c2"
        NETLIFY_AUTH_TOKEN = credentials("netlify-id")
    }

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
                    echo "Thisd is the Build stage"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Tests') {
            parallel {
                stage('Unit tests') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }

                    steps {
                        sh '''
                            echo "Thisd is the Tests stage"
                            #test -f build/index.html
                            npm test
                        '''
                    }
                    // post {
                    //     always {
                    //         junit 'jest-results/junit.xml'
                    //     }
                    // }
                }

                stage('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }

                    steps {
                        sh '''
                            echo "This is the E2E stage Testing"
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test  --reporter=html
                        '''
                    }

                    // post {
                    //     always {
                    //         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    //     }
                    // }
                }
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
                    echo "This is the Deployment Stage"
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "This is the Deployment of the site $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        }
    }
}
