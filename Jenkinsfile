pipeline {
    agent any

    environment{
        SONAR_TOKEN = "15771b5e67dfbcf6d6cf73049e646e1c14f9c464"
    }
    stages {
        stage('Build') {
            steps {
                echo "Build the code using Maven.."
                bat "mvn clean package"
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo "Run unit tests..."
                bat 'mvn test'
                echo "Run integration tests..."
                bat 'mvn integration-test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Analyze the code using SonarQube
                // bat 'sonar-scanner'
                bat "mvn sonar:sonar -Dsonar.token=${SONAR_TOKEN}"
            }
        }
        stage('Security Scan') {
            steps {
                // Perform security scan using OWASP ZAP
                bat 'zap-cli --quick-scan --spider --self-contained http://localhost:8080/myapp'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deploy the application to staging server (e.g., AWS EC2)
                bat 'ssh user@staging-server "cd /path/to/app && ./deploy.sh"'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Run integration tests on staging environment
                bat 'curl http://staging-server:8080/integration-tests'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deploy the application to production server (e.g., AWS EC2)
                bat 'ssh user@production-server "cd /path/to/app && ./deploy.sh"'
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
