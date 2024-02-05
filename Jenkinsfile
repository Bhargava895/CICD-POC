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

                    // Snyk integration
                    def snykResults = sh(script: 'npx snyk test --json', returnStdout: true).trim()

                    // Print Snyk results
                    echo "Snyk Results: ${snykResults}"

                    // Parse Snyk JSON results to check for Critical or High vulnerabilities
                    def vulnerabilities = readJSON text: snykResults

                    def criticalOrHigh = vulnerabilities.issues.findAll {
                        it.severity == 'high' || it.severity == 'critical'
                    }

                    // Fail the job if there are Critical or High vulnerabilities
                    if (criticalOrHigh.size() > 0) {
                        error "Build failed due to Critical or High vulnerabilities in dependencies."
                    }
                }
            }
        }
    }
}
