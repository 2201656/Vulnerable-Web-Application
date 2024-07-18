pipeline {
    agent any
    
    environment {
        // Define the SonarQube server URL and token
        SONARQUBE_SERVER = 'http://localhost:9000' // Replace with your SonarQube server URL
        SONARQUBE_TOKEN = credentials('sqp_c6ebe12150c76d10cb0a12c2dede396be0ad9b4') // Replace with the ID of your SonarQube token credential in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/2201656/Vulnerable-Web-Application.git'
            }
        }
        
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${env.SONARQUBE_SERVER} \
                        -Dsonar.login=${env.SONARQUBE_TOKEN}
                        """
                    }
                }
            }
        }
    }
    
    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}
