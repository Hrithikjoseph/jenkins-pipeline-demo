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
}
post {
success {
script {
mail to: 'manyamahajan3003@gmail.com',
subject: "Unit and Integration Tests Successful -
${currentBuild.fullDisplayName}",
body: "The unit and integration tests were successful.",
attachLog: true
}
}
failure {
script {
mail to: 'manyamahajan3003@gmail.com',
subject: "Unit and Integration Tests Failed -
${currentBuild.fullDisplayName}",
body: "The unit and integration tests failed.",
attachLog: true
}
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
}
post {
success {
script {
mail to: 'manyamahajan3003@gmail.com',
subject: "Security Scan Successful - ${currentBuild.fullDisplayName}",
body: "The security scan was successful.",
attachLog: true
}
}
failure {
script {
mail to: 'manyamahajan3003@gmail.com',
subject: "Security Scan Failed - ${currentBuild.fullDisplayName}",
body: "The security scan failed.",
attachLog: true
}
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
mail to: 'manyamahajan3003@gmail.com',
subject: "Jenkins Build Successful - ${currentBuild.fullDisplayName}",
body: "The build was successful.",
attachLog: true
}
}
failure {
script {
mail to: 'manyamahajan3003@gmail.com',
subject: "Jenkins Build Failed - ${currentBuild.fullDisplayName}",
body: "The build failed.",
attachLog: true
}
}
}
}
