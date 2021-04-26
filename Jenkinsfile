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
        stage ("OWASP-ZAP"){
			steps{
			    sh "docker run -p 9085:8080 -d  --name webgoat -e TZ=Europe/Amsterdam webgoat/webgoat-8.0"
				sh "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://192.168.18.23:9085/ || true" 
				sh "docker stop webgoat | xargs docker rm"
			}
		}
	stage('Build'){
	    steps{
		echo 'Done'
	    }
	}       
    }
}

