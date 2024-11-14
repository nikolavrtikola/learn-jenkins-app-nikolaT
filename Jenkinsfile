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
                echo 'Hello World Nikola commint on Github!'
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
            steps {
                echo 'Hello World From the  test stage'
            }
      
    }
}
