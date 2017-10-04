pipeline {
    agent { docker 'ruby' }
    stages {
        stage('build') {
            steps {
                sh 'ruby --version'
            }
        }
    }

    post {
    success {
        slackSend channel: '#jenkins',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
    failure {
        slackSend channel: '#jenkins',
                  color: 'danger',
                  message: "The pipeline ${currentBuild.fullDisplayName} failed."
        }
    }
}