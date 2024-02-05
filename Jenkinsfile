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

                    // Run Trivy vulnerability scanner in repo mode
                    // (Add steps as per your requirement)

                    // Run unit tests using maven goal
                    sh 'mvn test'

                    // Generate code-coverage report using jacoco
                    sh 'mvn jacoco:report'

                    // Upload code-coverage report as an artifact
                    archiveArtifacts 'target/site/jacoco/index.html'

                    // Build project and package jar
                    sh 'mvn package'
                }
            }
        }

      
    }
}
