import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import groovy.json.JsonOutput;

def notifySlack(status, color) {
    JSONArray attachments = new JSONArray();
    JSONObject attachment = new JSONObject();

    def payload = JsonOutput.toJson([
                    [
                        color: color,
                        "mrkdwn_in": ["fields"],
                        fields: [
                            [
                                title: "Topic",
                                value: "Puppet Control Repository Build Pipeline",
                                short: false
                            ],
                            [
                                title: "Build Status",
                                value: status,
                                short: true
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
                  message: 'N/A',
                  attachments: notifySlack('success', 'good')
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  message: 'N/A',
                  attachments: notifySlack('failure', 'danger')
        }
    }
}