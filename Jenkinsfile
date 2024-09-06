pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project.'
                bat 'mvn clean package' // Use 'bat' instead of 'sh' for Windows
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests.'
                bat 'mvn test'
            }
            post {
                always {
                    emailext(
                        to: 'sevin.dinsara@gmail.com',
                        subject: "Unit and Integration Tests: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                        body: "Unit and Integration Tests have been executed. Please check the results."
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing Code Analysis..'
                bat 'sonar-scanner -Dsonar.projectKey=my_project -Dsonar.sources=src'
            }
            post {
                always {
                    emailext(
                        to: 'sevin.dinsara@gmail.com',
                        subject: "Code Analysis: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                        body: "Code Analysis has been executed. Please check the results."
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing Security Scan...'
                bat 'dependency-check.bat --project my_project --out . --scan ./src'
            }
            post {
                always {
                    emailext(
                        to: 'sevin.dinsara@gmail.com',
                        subject: "Security Scan: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                        body: "Security Scan has been executed. Please check the results."
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                bat 'Copy-Item target' // Replace this with the correct copy command for Windows
            }
            post {
                always {
                    emailext(
                        to: 'sevin.dinsara@gmail.com',
                        subject: "Deploy to Staging: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                        body: "Deployment to Staging has been executed. Please check the results."
                    )
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging...'
                bat 'Invoke-WebRequest -Uri http://staging-server/api/tests -UseBasicParsing' // Ensure this command is valid for Windows
            }
            post {
                always {
                    emailext(
                        to: 'sevin.dinsara@gmail.com',
                        subject: "Integration Tests on Staging: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                        body: "Integration Tests on Staging have been executed. Please check the results."
                    )
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production...'
                bat 'Copy-Item target' // Replace this with the correct command for Production deployment
            }
            post {
                always {
                    emailext(
                        to: 'sevin.dinsara@gmail.com',
                        subject: "Deploy to Production: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                        body: "Deployment to Production has been executed. Please check the results."
                    )
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }

        success {
            emailext(
                attachLog: true, // Attach the log to the email
                to: 'sevin.dinsara@gmail.com',
                subject: "SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "The pipeline completed successfully."
            )
        }

        failure {
            echo 'Build failed!'
            emailext(
                attachLog: true, // Attach the log in case of failure
                to: 'sevin.dinsara@gmail.com',
                subject: "FAILURE: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: "The pipeline failed. Please check the logs for details."
            )
        }
    }
}

