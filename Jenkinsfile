pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Checkout code
                    checkout scm

                    // Set up JDK 17
                    tool 'java'
                    tool 'maven'

                    // Run unit tests using maven goal
                    sh 'mvn install -DskipTests'
                }
            }
        }

        stage('Scan') {
            steps {
                script {
                    def projectName = 'employee-management'
                    def severity = 'high,critical' 
                    def snykInstallation = 'snyk'
                    def snykTokenId = 'snyk-token-id'
                    def targetFile = 'pom.xml'

                    // snykSecurity projectName: projectName,
                    //             severity: severity,
                    //             snykInstallation: snykInstallation,
                    //             snykTokenId: snykTokenId,
                    //             targetFile: targetFile
                }
            }
        }
    }
}
