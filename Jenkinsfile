pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '4b1aa31c-0ca6-45c5-a684-4c19b151f849'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages{

        
        stage("Bild"){

            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci 
                    npm run build
                '''
            }
        }
        
        
        stage("Test"){
            parallel{
                stage("UnitTest"){

                    agent{
                        docker{
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }

                    steps{
                        sh '''
                            echo "Testing stage"
                            test -f build/index.html
                            npm test
                        '''
                    }
                }

                stage("E2E-Test"){
                    agent{
                        docker{
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }

                    steps{
                        sh '''
                            echo "E2E test stage"
                            npm install serve 
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                    }
                }

            }
        }

        stage("Deploy"){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deployinh to site ID : $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod 
                '''

            }
        }
        
    }

    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}