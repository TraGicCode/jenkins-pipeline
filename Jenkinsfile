def notifySlack(text, channel, attachments) {
    def jenkinsIcon = 'https://wiki.jenkins-ci.org/download/attachments/2916393/logo.png'

    def payload = JsonOutput.toJson([text: text,
        channel: channel,
        username: "Jenkins",
        icon_url: jenkinsIcon,
        attachments: attachments
    ])
   return payload
}

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
                  icon_url: 'https://wiki.jenkins-ci.org/download/attachments/2916393/logo.png',
                  attachments: notifySlack("", slackNotificationChannel, [
            [
                title: "${env.JOB_NAME}, build #${env.BUILD_NUMBER}",
                title_link: "${env.BUILD_URL}",
                color: "danger",
                author_name: "${author}",
                text: "${buildStatus}",
                fields: [
                    [
                        title: "Branch",
                        value: "${env.GIT_BRANCH}",
                        short: true
                    ],
                    [
                        title: "Test Results",
                        value: "${testSummary}",
                        short: true
                    ],
                    [
                        title: "Last Commit",
                        value: "${message}",
                        short: false
                    ],
                    [
                        title: "Error",
                        value: "${e}",
                        short: false
                    ]
                ]
            ]
        ])
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "075c0433-789a-48af-a1ec-d0d2411acbec"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  icon_url: 'https://wiki.jenkins-ci.org/download/attachments/2916393/logo.png',
                  attachments: notifySlack("", slackNotificationChannel, [
            [
                title: "${env.JOB_NAME}, build #${env.BUILD_NUMBER}",
                title_link: "${env.BUILD_URL}",
                color: "danger",
                author_name: "${author}",
                text: "${buildStatus}",
                fields: [
                    [
                        title: "Branch",
                        value: "${env.GIT_BRANCH}",
                        short: true
                    ],
                    [
                        title: "Test Results",
                        value: "${testSummary}",
                        short: true
                    ],
                    [
                        title: "Last Commit",
                        value: "${message}",
                        short: false
                    ],
                    [
                        title: "Error",
                        value: "${e}",
                        short: false
                    ]
                ]
            ]
        ])
    }
}