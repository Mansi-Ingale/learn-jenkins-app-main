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
            steps{
                sh '''
                    echo "Testing stage"
                '''
            }
        }
    }
}