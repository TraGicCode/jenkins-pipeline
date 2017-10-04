pipeline {
    agent any
    stages {
        stage('Build') {
            steps {

                githubNotify status: "PENDING", description: "Build is starting...", credentialsId: "a192d405-f076-4417-a771-e81b459003e3"
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