pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Build the code using Maven.."
                sh "mvn clean package"
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Run unit tests
                sh 'mvn test'
                // Run integration tests
                sh 'mvn integration-test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Analyze the code using SonarQube
                sh 'sonar-scanner'
            }
        }
        stage('Security Scan') {
            steps {
                // Perform security scan using OWASP ZAP
                sh 'zap-cli --quick-scan --spider --self-contained http://localhost:8080/myapp'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deploy the application to staging server (e.g., AWS EC2)
                sh 'ssh user@staging-server "cd /path/to/app && ./deploy.sh"'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Run integration tests on staging environment
                sh 'curl http://staging-server:8080/integration-tests'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deploy the application to production server (e.g., AWS EC2)
                sh 'ssh user@production-server "cd /path/to/app && ./deploy.sh"'
            }
        }
    }
    
    post {
        success {
            // Send success notification email with logs attached
            emailext (
                to: 'morismutea@gmail.com',
                subject: 'Pipeline Success',
                body: 'Pipeline ran successfully.',
                attachmentsPattern: '**/*.log'
            )
        }
        failure {
            // Send failure notification email with logs attached
            emailext (
                to: 'morismutea@gmail.com',
                subject: 'Pipeline Failure',
                body: 'Pipeline failed.',
                attachmentsPattern: '**/*.log'
            )
        }
    }
}
