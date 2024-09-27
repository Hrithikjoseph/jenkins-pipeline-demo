pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Example build tool: Maven
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Example test tool: JUnit
                script {
                    emailext(
                        to: 'manyamahajan3003@gmail.com',
                        subject: "Build ${currentBuild.fullDisplayName} - Unit and Integration Tests",
                        body: """<p>The Unit and Integration Tests stage has completed successfully.</p>
                                <p>Check the attached logs for details.</p>""",
                        attachLog: true
                    )
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code quality...'
                // Example analysis tool: SonarQube
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Example security tool: OWASP Dependency-Check
                script {
                    emailext(
                        to: 'manyamahajan3003@gmail.com',
                        subject: "Build ${currentBuild.fullDisplayName} - Security Scan",
                        body: """<p>The Security Scan stage has completed successfully.</p>
                                <p>Check the attached logs for details.</p>""",
                        attachLog: true
                    )
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Example deployment tool: AWS CLI
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Example test tool: Selenium
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Example deployment tool: AWS CLI
            }
        }
    }
    post {
        success {
            script {
                emailext(
                    to: 'manyamahajan3003@gmail.com',
                    subject: "Jenkins Build Successful - ${currentBuild.fullDisplayName}",
                    body: """<p>The build was successful.</p><p>Check the attached logs for details.</p>""",
                    attachLog: true
                )
            }
        }
        failure {
            script {
                emailext(
                    to: 'manyamahajan3003@gmail.com',
                    subject: "Jenkins Build Failed - ${currentBuild.fullDisplayName}",
                    body: """<p>The build has failed.</p><p>Check the attached logs for details.</p>""",
                    attachLog: true
                )
            }
        }
    }
}
