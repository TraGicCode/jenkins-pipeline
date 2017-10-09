import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
def notifySlack() {
    JSONArray attachments = new JSONArray();
    JSONObject attachment = new JSONObject();

    attachment.put('text','I find your lack of faith disturbing!');
    attachment.put('fallback','Hey, Vader seems to be mad at you.');
    attachment.put('color','#ff0000');

    attachments.add(attachment);
    return attachments
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
                  attachments: notifySlack
        }
    failure {
        githubNotify status: "FAILURE", description: "The Jenkins CI build failed.", credentialsId: "075c0433-789a-48af-a1ec-d0d2411acbec"
        slackSend channel: '#jenkins',
                  baseUrl: 'https://hooks.slack.com/services',
                  color: 'danger',
                  attachments: notifySlack.toString()
        }
    }
}