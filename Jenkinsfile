pipeline {
    agent any
    
    environment {
        // Define the SonarQube server URL and token
        SONARQUBE_SERVER = 'http://localhost:9000/'
        SONARQUBE_TOKEN = 'sqp_a1a3c0ce38d28d8e7cd1125981a488a26eca0861'
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
                    def scannerHome = tool 'SonarDocker';
                    withSonarQubeEnv('SonarDocker') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${env.SONARQUBE_SERVER} \
                        -Dsonar.login=${env.SONARQUBE_TOKEN} \
                        -Dsonar.verbose=true -X
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
