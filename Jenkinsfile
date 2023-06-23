pipeline {
    agent {
        label 'docker'
    }
    environment {
        GCP_SERVICE_ACCOUNT = credentials('gcp_service_account_storage')
        DISCORD = credentials('discord')
    }
    parameters {
        string(name: 'TAG', defaultValue: 'Alpha', description: 'Tag for your Image')
    }
    stages {
        stage('Build') {
            steps {
                echo 'build App'
                sh 'docker build -t gcr.io/studi-devops-370205/goapp:${TAG} .'
            }
        }
        stage('Push To GCR') {
            steps {
                echo 'Push Image'
                sh 'cat "$GCP_SERVICE_ACCOUNT" | docker login -u _json_key --password-stdin https://gcr.io'
                sh 'docker push gcr.io/studi-devops-370205/goapp:${TAG}'
            }
            post {
                failure {
                    echo "post failure"
                    discordSend description: "Jenkins Build And Push Image goapp:${TAG}", footer: "post failure", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$DISCORD"
                }
                success {
                    echo "post success"
                    discordSend description: "Jenkins Build And Push Image goapp:${TAG}", footer: "post success", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$DISCORD"
                }
                aborted {
                    echo "post aborted"
                    discordSend description: "Jenkins Build And Push Image goapp:${TAG}", footer: "post aborted", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$DISCORD"
                }
            }
        }
      
    }
}
