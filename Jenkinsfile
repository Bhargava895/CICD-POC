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

        stage('Install Snyk CLI') {
            steps {
                 script {
                   def projectName = 'employee-management'
                   def severity = 'high,critical' 
                   def snykInstallation = 'snyk'
                   def snykTokenId = 'snyk-token-id'
                   def targetFile = 'pom.xml'
                     
                // Download Snyk CLI
                sh 'curl --compressed https://static.snyk.io/cli/latest/snyk-macos -o snyk'

                // Make Snyk CLI executable
                sh 'chmod +x ./snyk'

                // Move Snyk CLI to /usr/local/bin
                sh 'sudo mv ./snyk /usr/local/bin/'
                // Authenticate with Snyk
                sh "snyk auth ${snykTokenId}"

                // Run Snyk Code scan (optionally filter severity and scope)
                sh 'snyk code test --severity-threshold=high --all-projects --maven'

                // Run Snyk Container Scan (if applicable)
                // sh 'snyk container test image=${REGISTRY}/${IMAGE_NAME}:${BUILD_SHA}' // Replace with image details

                // Run Snyk Infrastructure as Code (IaC) scan (if applicable)
                sh 'snyk iac test file=./manifests/deployment.yaml' // Replace with file path
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
         stage('Snyk Security Scan') {
            steps {
                script {
                   def projectName = 'employee-management'
                   def severity = 'high,critical' 
                   def snykInstallation = 'snyk'
                   def snykTokenId = 'snyk-token-id'
                   def targetFile = 'pom.xml'

                // Set up Snyk CLI to check for security issues
                sh "curl -sL https://raw.githubusercontent.com/snyk/snyk/master/tools/setup.sh | bash -s -- -t ${snykTokenId}"

                // Authenticate with Snyk
                sh "snyk auth ${snykTokenId}"

                // Run Snyk Code scan (optionally filter severity and scope)
                sh 'snyk code test --severity-threshold=high --all-projects --maven'

                // Run Snyk Container Scan (if applicable)
                // sh 'snyk container test image=${REGISTRY}/${IMAGE_NAME}:${BUILD_SHA}' // Replace with image details

                // Run Snyk Infrastructure as Code (IaC) scan (if applicable)
                sh 'snyk iac test file=./manifests/deployment.yaml' // Replace with file path
            }
          }
        }
    }
}
