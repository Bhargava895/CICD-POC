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

                    // Run Trivy vulnerability scanner in repo mode
                    // (Add steps as per your requirement)

                    // Run unit tests using maven goal
                    sh 'mvn install -DskipTests'

                    // Generate code-coverage report using jacoco
                    sh 'mvn jacoco:report'

                    // Upload code-coverage report as an artifact
                    archiveArtifacts 'target/site/jacoco/index.html'
   
            }
        }
    }

        stage('Scan') {
            steps {
                script {
                    def organization = 'cybage-poc'
                    def projectName = 'employee-management'
                    def severity = 'high,critical' 
                    def snykInstallation = 'snyk'
                    def snykTokenId = 'snyk-token-id'                   
                    def targetFile = 'pom.xml'

                    snykSecurity organization: organization,
                                 projectName: projectName,
                                 severity: severity,
                                 snykInstallation: snykInstallation,
                                 snykTokenId: snykTokenId,
                                 targetFile: targetFile
                }
            }
        }
    }
}
 
           
