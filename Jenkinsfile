pipeline {
    agent any
    tools {
        maven 'mvn-3.61'
        jdk 'Open JDK'
        nodejs 'nodejs-12'
    }
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
        stage('Sonarqube'){
            steps{
                withSonarQubeEnv('default') {
                     sh "${tool("sonar-tool")}/bin/sonar-scanner \
                     -Dsonar.projectKey=webgoat-proj \
                     -Dsonar.sources=.\
                     -Dsonar.exclusions=**/*.java"
                }
            }
        }
	stage('Build'){
	    steps{
		echo 'Done'
	    }
	}       
    }
}

