pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building the code using Maven...'
                // Tool: Maven
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit...'
                // Tools: JUnit, TestNG, etc.
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code using SonarQube...'
                // Tool: SonarQube
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Scanning code for vulnerabilities using OWASP ZAP...'
                // Tool: OWASP ZAP
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to AWS EC2 Staging server...'
                // Target: AWS EC2 Staging
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                // Tools: Selenium, Cucumber, etc.
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to AWS EC2 Production server...'
                // Target: AWS EC2 Production
            }
        }
    }
}
