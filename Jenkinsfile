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
                echo 'Running unit and integration tests using TestNG...'
                // Tools: JUnit, TestNG,etc..
                script {
                    currentBuild.result = 'SUCCESS' 
                    emailext(
                        to: 'hrithikjsoeph72@gmail.com',
                        subject: "Build ${currentBuild.fullDisplayName} - Test Stage",
                        body: """<p>The Test stage has completed with status: ${currentBuild.result}.</p>""",
                        attachLog: true
                    )
                }
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
                script {
                    currentBuild.result = 'SUCCESS' 
                    emailext(
                        to: 'hrithikjsoeph72@gmail.com',
                        subject: "Build ${currentBuild.fullDisplayName} - Security Scan Stage",
                        body: """<p>The Security Scan stage has completed with status: ${currentBuild.result}.</p>""",
                        attachLog: true
                    )
                }
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
