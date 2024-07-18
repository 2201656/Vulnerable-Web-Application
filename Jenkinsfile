pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/2201656/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_c6ebe12150c76d10cb0a12c2dede396be0ad9b4"
                    }
                }
            }
        }
    }
    post {
        always {
            recordIssues tools: [java()]
        }
    }
}
