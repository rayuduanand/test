pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                catchError {
                    checkout scm

                    script {
                        // Your existing stages go here
                        stage('Checkout') {
                            checkout scm
                        }

                        stage('Build') {
                            mvn 'clean install -DskipTests'
                        }

                        stage('Unit Test') {
                            mvn 'test'
                        }

                        stage('Integration Test') {
                            mvn 'verify -DskipUnitTests -Parq-wildfly-swarm'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                def previousBuild = currentBuild.previousBuild
                if (previousBuild != null && previousBuild.result != currentBuild.currentResult) {
                    mail to: "${env.EMAIL_RECIPIENTS}",
                         subject: "${JOB_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}!",
                         body: "Check console output at ${BUILD_URL} to view the results."
                }
            }
        }
    }
}
