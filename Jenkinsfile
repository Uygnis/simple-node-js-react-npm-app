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
        stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'

                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}