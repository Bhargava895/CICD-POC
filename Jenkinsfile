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
              }
            }
          }
        stage('Run Unit Test Cases') {
            steps {
                script {
                    sh "mvn test"
                }
             }
          }
        stage('Generate Code Coverage Report') {
            steps {
                script {
                    // Generate code-coverage report using Jacoco
                    sh 'mvn jacoco:report'
                }
            }
            post {
                always {
                    // Upload code-coverage report as an artifact
                    archiveArtifacts artifacts: 'target/site/jacoco/index.html', onlyIfSuccessful: true
                }
            }
        }
        stage('Build project and package jar') {
            steps {
                script {
                    // This command builds the Maven project and packages it into a JAR
                    sh 'mvn package'
                }
            }

            post {
                success {
                    // This block is executed if the build is successful
                    echo 'Build successful!'
                }

                failure {
                    // This block is executed if the build fails
                    echo 'Build failed!'
                }
            }
        }
       stage('Build Image') {
            steps {
                script {
                    // Retrieve the commit SHA from the Jenkins environment
                    def shortCommitSha = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
        
                    // Build the image with the commit SHA as the tag
                    sh "docker build -t employeemangement:${shortCommitSha} ."  
                }
            }
        }
        
                    // // Run unit tests using maven goal
                    // sh 'mvn install'

                    // // Build Docker image
                    // sh 'docker build -t myapp .'
                

         stage('Snyk Scan') {
            steps {
                withCredentials([string(credentialsId: 'snyk-token-id', variable: 'SNYK_TOKEN')]) {
                    sh "snyk auth $SNYK_TOKEN" // Authenticate with Snyk using the stored token
                    // sh "snyk code test" // Run Snyk test for vulnerabilities in source code
                   // sh "snyk test --all-projects --maven" //Run snyk test for vulnerabilites in pom.xml
                    
                    // sh 'snyk test myapp' // Run Snyk test for vulnerabilities on the Docker image
                    sh "snyk iac test ./manifests/*.yaml"

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
