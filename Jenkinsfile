pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project.'
                sh 'mvn clean package' // Replace with 'bat' if using Windows
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests.'
                sh 'mvn test'
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
                sh 'sonar-scanner -Dsonar.projectKey=my_project -Dsonar.sources=src'
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
                sh 'dependency-check.bat --project my_project --out . --scan ./src'
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
                sh 'Copy-Item target' // Use the correct command depending on your environment
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
                sh 'Invoke-WebRequest -Uri http://staging-server/api/tests -UseBasicParsing' // Ensure the correct syntax is used for your environment
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
                sh 'Copy-Item target' // Replace with the correct command for production deployment
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
