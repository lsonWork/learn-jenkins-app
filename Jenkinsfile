pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'b5d72e02-88bc-4860-98fd-94643803d3cd'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
                    echo 'Small change'
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
                            #test -f build/index.html
                            npm test
                        '''
                    }
                    post {
                        always {
                            junit 'jest-results/junit.xml'
                        }
                    }
                }

                stage('E2E') {
                    // agent {
                    //     docker {
                    //         image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    //         reuseNode true
                    //     }
                    // }

                    // steps {
                    //     sh '''
                    //         npm install serve
                    //         node_modules/.bin/serve -s build &
                    //         sleep 10
                    //         npx playwright test  --reporter=html
                    //         echo 'This is E2E'
                    //     '''
                    // }

                    // post {
                    //     always {
                    //         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright Local', reportTitles: '', useWrapperFileDirectly: true])
                    //     }
                    // }
                    steps {
                        sh '''
                            echo 'demo e2e'
                        '''
                    }
                }
            }
        }

        stage('Deploy staging') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to staging. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build
                '''
            }
        }

        stage('Deploy prod') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }

        stage('Prod E2E') {
            // agent {
            //     docker {
            //         image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
            //         reuseNode true
            //     }
            // }

            // environment {
            //     CI_ENVIRONMENT_URL = 'https://heroic-capybara-db9b01.netlify.app'
            // }

            // steps {
            //     sh '''
                    
            //         npx playwright test  --reporter=html
            //     '''
            // }

            // post {
            //     always {
            //         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright E2E', reportTitles: '', useWrapperFileDirectly: true])
            //     }
            // }
            steps {
                sh '''
                    echo 'demo prod e2e'
                '''
            }
        }
    }
}
