pipeline {
    agent any
    stages {
        stage('Build') {
            githubNotify(
                context:     'Jenkins Build',
                description: 'Build has been scheduled.',
                status:      'PENDING',
                credentialsId: 'tragiccode/******',
                account: 'tragiccode',
                repo: 'jenkins-pipeline',
                targetUrl: 'http://www.cloudbees.com'
            )
            steps {
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