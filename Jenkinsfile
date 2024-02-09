pipeline {
    agent any

    environment {
     SNYK_TOKEN = credentials('snyk-token-id')
    }
    
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
                    sh 'mvn install'

                    // Build Docker image
                    sh 'docker build -t myapp .'
                }
            }
        }
         stage('Snyk Scan') {
            steps {
                withCredentials([string(credentialsId: 'snyk-token-id', variable: 'SNYK_TOKEN')]) {
                    sh "snyk auth $SNYK_TOKEN" // Authenticate with Snyk using the stored token
                    // sh "snyk code test" // Run Snyk test for vulnerabilities in source code
                   // sh "snyk test --all-projects --maven" //Run snyk test for vulnerabilites in pom.xml
                    
                    // sh 'snyk test myapp' // Run Snyk test for vulnerabilities on the Docker image
                    sh "snyk iac"


                }
            }
        }

    
        // stage('Scan') {
        //     steps {
        //         script {
        //             def projectName = 'employee-management'
        //             def severity = 'high,critical' 
        //             def snykInstallation = 'snyk'
        //             def snykTokenId = 'snyk-token-id'
        //             def targetFile = 'pom.xml'

        //             // snykSecurity projectName: projectName,
        //             //             severity: severity,
        //             //             snykInstallation: snykInstallation,
        //             //             snykTokenId: snykTokenId,
        //             //             targetFile: targetFile
        //         }
        //     }
        // }
    }
}
