pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project.'
                sh 'mvn clean package' 
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests.'
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing Code Analysis...'
                sh 'sonar-scanner -Dsonar.projectKey=my_project -Dsonar.sources=src'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing Security Scan...'
                sh 'dependency-check.bat --project my_project --out . --scan ./src'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                sh 'Copy-Item target/my-app.jar -Destination \\\\staging-server\\path\\to\\deploy -Force'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging...'
                sh 'Invoke-WebRequest -Uri http://staging-server/api/tests -UseBasicParsing'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }

        success {
            echo 'Build succeeded!'
            mail to: 'sevin.dinsara@gmail.com',
                 subject: "SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The pipeline completed successfully."
        }

        failure {
            echo 'Build failed!'
            mail to: 'sevin.dinsara@gmail.com',
                 subject: "FAILURE: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The pipeline failed. Please check the logs for details."
        }
    }
}
