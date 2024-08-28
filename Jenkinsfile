pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the code...'
                // Use a build automation tool like Maven or Gradle
                sh 'mvn clean install'  // Example for Maven
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Use testing tools like JUnit
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                // Use code analysis tools like SonarQube
                sh 'sonar-scanner'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Use security tools like OWASP Dependency-Check
                sh 'dependency-check --project pipeline --scan .'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Deploy to staging environment (e.g., AWS EC2)
                sh 'scp -i my-key.pem target/*.jar ec2-user@staging-server:/path/to/deploy'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Run tests in the staging environment
                sh 'curl -X GET http://staging-server/api/health'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Deploy to production (e.g., AWS EC2)
                sh 'scp -i my-key.pem target/*.jar ec2-user@production-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            mail to: 'developer@example.com',
                 subject: "SUCCESS: Pipeline ${currentBuild.fullDisplayName}",
                 body: "Pipeline succeeded: ${currentBuild.fullDisplayName}"
        }
        failure {
            mail to: 'developer@example.com',
                 subject: "FAILURE: Pipeline ${currentBuild.fullDisplayName}",
                 body: "Pipeline failed: ${currentBuild.fullDisplayName}",
                 attachmentsPattern: '**/target/**/*.log'
        }
    }
}
