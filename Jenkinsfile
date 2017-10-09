import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import groovy.json.JsonOutput
def notifySlack() {
    JSONArray attachments = new JSONArray();
    JSONObject attachment = new JSONObject();

    def payload = JsonOutput.toJson([
        attachments:     [
                    [
                        title: "asd, build #${env.BUILD_NUMBER}",
                        title_link: "${env.BUILD_URL}",
                        color: "red",
                        text: "asd",
                        "mrkdwn_in": ["fields"],
                        fields: [
                            [
                                title: "Branch",
                                value: "${env.GIT_BRANCH}",
                                short: true
                            ],
                            [
                                title: "Test Results",
                                value: "asd",
                                short: true
                            ],
                            [
                                title: "Last Commit",
                                value: "asd",
                                short: false
                            ]
                        ]
                    ],
                    [
                        title: "Failed Tests",
                        color: "red",
                        text: "asd",
                        "mrkdwn_in": ["text"],
                    ]
                ]
    ])

    return payload
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                githubNotify status: "PENDING", description: "Build is starting...", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
                echo 'Building'
                sh 'sleep 1'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'sleep 1'
            }
        }
        stage('Deploy') {
            when { branch "production" }
            steps {
                echo 'Deploying'
                sh 'sleep 1'
            }
        }
    }

    post {
    success {
        githubNotify status: "SUCCESS", description: "The Jenkins CI build passwed.", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'good',
                  message: 'asd',
                  attachments: notifySlack()
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  message: 'asd',
                  attachments: notifySlack()
        }
    }
}