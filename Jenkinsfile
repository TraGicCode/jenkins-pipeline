pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                githubNotify status: "PENDING", description: "Build is starting...", credentialsId: "075c0433-789a-48af-a1ec-d0d2411acbec"
                echo 'Building'
                sh 'sleep 10'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'sleep 10'
            }
        }
        stage('Deploy') {
            when { branch "production" }
            steps {
                echo 'Deploying'
                sh 'sleep 10'
            }
        }
    }

    post {
    success {
        githubNotify status: "SUCCESS", description: "The Jenkins CI build passwed.", credentialsId: "075c0433-789a-48af-a1ec-d0d2411acbec"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "075c0433-789a-48af-a1ec-d0d2411acbec"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  message: "The pipeline ${currentBuild.fullDisplayName} failed."
        }
    }
}