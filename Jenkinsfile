pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                
                sh 'mvn clean package' 
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests...'
                
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
               
                sh 'dependency-check.sh --project my_project --out . --scan ./src'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                
                sh 'scp target/my-app.jar user@staging-server:/path/to/deploy' 
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging...'
                
                sh 'curl -X GET http://staging-server/api/tests' 
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production...'
              
                sh 'scp target/my-app.jar user@production-server:/path/to/deploy'
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
