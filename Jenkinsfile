pipeline {
    agent any

    stages {
        stage('Git Pull') {
            steps {
                git 'https://github.com/WebGoat/WebGoat'
            }
        }
        stage('OWASP Deps Checker') {
            steps {
                dependencyCheck additionalArguments: '', odcInstallation: 'Default'
            }
        }
        stage('OWASP Report') {
            steps {
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }        
        stage('Build') {
            steps {
                echo 'Done'
            }
        }        
    }
}

