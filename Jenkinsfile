pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                githubNotify status: "PENDING", description: "Build is starting...", credentialsId: "c3b1f63c-a0ed-43f5-9956-91cf09b830bb"
                echo 'Building'
                sh 'sleep 30'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'sleep 30'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                sh 'sleep 30'
            }
        }
    }

    post {
    success {
        githubNotify status: "SUCCESS", description: "The Jenkins CI build passwed.", credentialsId: "c3b1f63c-a0ed-43f5-9956-91cf09b830bb"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "c3b1f63c-a0ed-43f5-9956-91cf09b830bb"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  message: "The pipeline ${currentBuild.fullDisplayName} failed."
        }
    }
}