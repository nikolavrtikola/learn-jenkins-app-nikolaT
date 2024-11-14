pipeline {
    agent any

    stages {
        stage('Build') {
          agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Hello World Nikola commint on Github! Build Stage'
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

            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                echo 'Hello World From the  test stage ..neka ostane Test Stage!!!'
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
                    echo "From the deploy stage installing Netlify"
                    npm install netlify-cli -g
                    netlify --version
                '''
            }
        }            

    
    }

 
}
