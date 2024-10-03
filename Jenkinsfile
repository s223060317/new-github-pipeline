pipeline {
    agent any
    stages {
        stage("Build Stage") {
            steps {
                echo 'Executing Maven build: cleaning and packaging the project'
                // Running the Maven clean and package commands
                sh 'mvn clean package'
            }
        }
        stage("Testing: Unit & Integration") {
            steps {
                echo 'Running JUnit tests to validate code functionality'
                // Running unit tests using Maven
                sh 'mvn test'
                echo 'Running integration tests to verify component interactions'
                // Running integration tests using Maven
                sh 'mvn verify'
            }
            post {
                success {
                    emailext(
                        to: "dominicdiona@gmail.com",
                        subject: "SUCCESS: Unit and Integration Tests Passed",
                        body: "Both unit and integration tests were successful. The codebase is functioning correctly.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "dominicdiona@gmail.com",
                        subject: "ERROR: Unit and/or Integration Tests Failed",
                        body: "Unit or integration tests failed. Please check the attached logs for details on the failure.",
                        attachLog: true
                    )
                }
            }
        }
        stage("Code Quality Analysis") {
            steps {
                echo 'Starting SonarQube analysis for code quality assurance'
                // Running SonarQube analysis using Maven
                sh 'mvn sonar:sonar'
            }
        }
        stage("Security Assessment") {
            steps {
                echo 'Initiating security scan using OWASP ZAP'
                // Running OWASP ZAP for security scanning
                sh 'zap-cli quick-scan --self-contained --start-options "-config api.disablekey=true" http://localhost:8080'
            }
            post {
                success {
                    emailext(
                        to: "dominicdiona@gmail.com",
                        subject: "SUCCESS: Security Scan Completed",
                        body: "The security scan was successfully completed with no issues detected. Please review the attached logs for detailed information.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "dominicdiona@gmail.com",
                        subject: "ERROR: Security Scan Failed",
                        body: "The security scan failed. Please review the attached logs for details on the detected vulnerabilities or issues.",
                        attachLog: true
                    )
                }
            }
        }
        stage("Staging Deployment") {
            steps {
                echo 'Deploying application to the staging environment (AWS EC2, S3 bucket)'
                // Deploying to the staging environment
                sh 'scp target/*.jar user@staging-server:/path/to/deploy'
            }
        }
        stage("Staging Environment Tests") {
            steps {
                echo 'Executing integration tests on the staging environment'
                // Running integration tests in the staging environment
                sh 'run-integration-tests-on-staging'
            }
        }
        stage("Production Deployment") {
            steps {
                echo 'Deploying the application to the production environment'
                // Deploying to the production environment
                sh 'scp target/*.jar user@production-server:/path/to/deploy'
            }
        }
        stage("Finalization") {
            steps {
                echo 'Pipeline execution complete'
            }
        }
    }
    post {
        success {
            emailext(
                to: "dominicdiona@gmail.com",
                subject: "Production deployment successful!",
                body: "The application was successfully deployed to production. Check the attached log for details.",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: "dominicdiona@gmail.com",
                subject: "Production deployment failed!",
                body: "The production deployment failed. Review the attached logs for errors and retry the deployment after fixing the issues.",
                attachLog: true
            )
        }
    }
}
