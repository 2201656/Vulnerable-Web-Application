pipeline {
    agent any
    
    environment {
        // Define the SonarQube server URL and token
        SONARQUBE_SERVER = 'http://localhost:9000' // Adjusted SonarQube server URL
        SONARQUBE_TOKEN = 'sqp_c6ebe12150c76d10cb0a12c2dede396be0ad9b4' // SonarQube token
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
            // Ensure the 'recordIssues' step is inside a 'node' block with a label
            node {
                // Specify a label or use 'any' if you want to run on any available agent
                // Example: node('docker') {
                recordIssues enabledForFailure: true, tool: sonarQube()
                // }
            }
        }
    }
}
