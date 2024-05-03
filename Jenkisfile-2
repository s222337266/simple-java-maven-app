pipeline {
    agent any

    environment{
        SONAR_TOKEN = "sqa_15771b5e67dfbcf6d6cf73049e646e1c14f9c464"
    }
    stages {
        stage('Build') {
            steps {
                echo "Build the code using Maven.."
                echo 'Tool: Maven'
                echo "mvn clean package"
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo "Run unit tests using maven..."
                echo 'Tool: Maven'
                echo 'mvn test'
                echo "Run integration tests using maven integration test..."
                echo 'mvn integration-test'
            }
        }
        stage('Code Analysis') {
            steps {
                echo "Perfoming code analyses using SonarQube.."
                echo 'Tool: SonarQube'
                echo "mvn sonar:sonar -Dsonar.token=${SONAR_TOKEN}"            
            }
        }
        stage('Security Scan') {
            steps {
                echo "Perform security scan..."
                echo 'Tool: OWASP ZAP'
                echo "zap-cli --quick-scan --spider --self-contained http://localhost:8080/myapp"
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo "Deploy the application to staging server in AWS EC2..."
                echo 'Tool: Secure Shell (ssh)'
                echo 'ssh user@staging-server "cd /path/to/app && ./deploy.sh"'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo "Run integration tests on staging environment with selenium.."
                echo "Tool: Selenium"
            }
        }
        stage('Deploy to Production') {
            steps {
                echo "Deploy the application to production server in AWS EC2 instance..."
                echo 'Tool: Secure Shell (ssh)'
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
