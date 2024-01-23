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

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def reportTaskPath = "${JENKINS_HOME}/workspace/mysonardemo/.scannerwork/report-task.txt"
                        def ceTaskId = readFile(file: reportTaskPath).trim()

                        echo "ceTaskId: ${ceTaskId}"

                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}
