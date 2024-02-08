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
                   // sh 'mvn install -DskipTests'
                    sh 'mvn clean install snyk:test --all-projects'
                }
            }
        }
    

        // stage('Scan') {
        //     steps {
        //         script {
        //             def organization = 'cybage-poc'
        //             def projectName = 'employee-management'
        //             def severity = 'high,critical' 
        //             def snykInstallation = 'snyk'
        //             def snykTokenId = 'snyk-token-id'                   
        //             def targetFile = 'pom.xml'

        //             snykSecurity organization: organization,
        //                          projectName: projectName,
        //                          severity: severity,
        //                          snykInstallation: snykInstallation,
        //                          snykTokenId: snykTokenId,
        //                          targetFile: targetFile
        //         }
        //     }
        // }
       
    }
}
 
           
