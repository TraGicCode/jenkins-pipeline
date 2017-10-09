import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
def notifySlack() {
    JSONArray attachments = new JSONArray();
    JSONObject attachment = new JSONObject();

    def payload = JsonOutput.toJson([
        attachments:     [
                    [
                        title: "${jobName}, build #${env.BUILD_NUMBER}",
                        title_link: "${env.BUILD_URL}",
                        color: "${buildColor}",
                        text: "${buildStatus}\n${author}",
                        "mrkdwn_in": ["fields"],
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
                            ]
                        ]
                    ],
                    [
                        title: "Failed Tests",
                        color: "${buildColor}",
                        text: "${failedTestsString}",
                        "mrkdwn_in": ["text"],
                    ]
                ]
    ])

    attachments.add(payload);
    return attachments
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                githubNotify status: "PENDING", description: "Build is starting...", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
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
        githubNotify status: "SUCCESS", description: "The Jenkins CI build passwed.", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'good',
                  message: 'asd',
                  attachments: notifySlack().toString()
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "a01cf4f0-3b25-4e3c-a112-65f33111e93f"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  message: 'asd',
                  attachments: notifySlack().toString()
        }
    }
}