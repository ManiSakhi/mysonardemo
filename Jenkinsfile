pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonarqube'
                    withSonarQubeEnv() {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Test"
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'Minutes') {
                    waitForQualityGate failpipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! Ready for deployment.'
        }
        failure {
            echo 'Pipeline failed. Quality Gate check failed.'
        }
    }
}
