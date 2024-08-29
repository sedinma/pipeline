pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                
            
                bat 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests...'
                
                
                bat 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing Code Analysis...'
                
                
                bat 'sonar-scanner -Dsonar.projectKey=my_project -Dsonar.sources=src'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing Security Scan...'
               
                
                bat 'dependency-check.bat --project my_project --out . --scan ./src'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                
               
                powershell 'Copy-Item target/my-app.jar -Destination \\\\staging-server\\path\\to\\deploy -Force'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging...'
                
           
                powershell 'Invoke-WebRequest -Uri http://staging-server/api/tests -UseBasicParsing'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production...'
                
                
                powershell 'Copy-Item target/my-app.jar -Destination \\\\production-server\\path\\to\\deploy -Force'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            deleteDir() 
        }

        success {
            echo 'Build succeeded!'
            mail to: 'developer@example.com',
                 subject: "SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The pipeline completed successfully."
        }

        failure {
            echo 'Build failed!'
            mail to: 'developer@example.com',
                 subject: "FAILURE: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The pipeline failed. Please check the logs for details."
        }
    }
}
