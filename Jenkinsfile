pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
    }

    post {
    success {
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
    failure {
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  message: "The pipeline ${currentBuild.fullDisplayName} failed."
        }
    }
}