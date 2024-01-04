node {
    catchError {
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
            mvn 'verify -DskipUnitTests -Parq-wildfly-swarm '
        }
    }

    // Archive Unit and integration test results, if any
    junit allowEmptyResults: true,
          testResults: '**/target/surefire-reports/TEST-*.xml, **/target/failsafe-reports/*.xml'

    statusChanged {
        mail to: "${env.EMAIL_RECIPIENTS}",
             subject: "${JOB_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}!",
             body: "Check console output at ${BUILD_URL} to view the results."
    }
}

def statusChanged(body) {
    def previousBuild = currentBuild.previousBuild
    if (previousBuild != null && previousBuild.result != currentBuild.currentResult) {
        body()
    }
}
