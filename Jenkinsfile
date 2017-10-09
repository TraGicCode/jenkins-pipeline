import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import groovy.json.JsonOutput
def notifySlack() {
    JSONArray attachments = new JSONArray();
    JSONObject attachment = new JSONObject();

    def payload = JsonOutput.toJson([
                    [
                        author_name: 'jenkins',
                        color: "red",
                        "mrkdwn_in": ["fields"],
                        fields: [
                            [
                                title: "Topic",
                                value: "Puppet Control Repository Build Pipeline",
                                short: false
                            ],
                            [
                                title: "Branch",
                                value: "${env.BRANCH_NAME}",
                                short: true
                            ],
                            [
                                title: "Build Log",
                                value: "<${env.BUILD_URL}|Click Here>",
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