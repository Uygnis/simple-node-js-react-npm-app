pipeline {
    agent {
        docker {
            image 'node:18.18.1-alpine3.18' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
         stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                // Check if the server is running before proceeding
                script {
                    def serverIsRunning = sh(script: 'curl -Is http://localhost:3000 | head -n 1 | grep -q 200', returnStatus: true) == 0
                    if (!serverIsRunning) {
                        error('The server is not running. Cannot proceed.')
                    }
                }
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}