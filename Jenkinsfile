pipeline {
    agent any

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
    }
}