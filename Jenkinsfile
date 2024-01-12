pipeline {
    agent any
    tools{
        jdk  'jdk11'
        maven  'maven3'
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/RahulSinha9/Shopping-Cart'
            }
        }
        
        stage('compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        
        
        stage('Sonarqube Analysis') {
            steps {
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://18.234.226.160:9000/ -Dsonar.login=squ_eedcb995a8e2bced369520a2eecdbb47439b4d15 -Dsonar.projectName=Shopping-Cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               
            }
        }
        
        stage('OWASP Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Build Application') {
            steps {
                sh "mvn clean Install"
            }
        }
        
        
        
        
    }
}
