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
            mvn 'verify -DskipUnitTests -Parq-wildfly-swarm'
        }
    }
}
