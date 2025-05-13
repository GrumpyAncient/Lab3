pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    environment {
        TEAMS_WEBHOOK_URL = 'https://lpnu.webhook.office.com/...' // скорочено
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        always {
            script {
                def status = currentBuild.currentResult
                def emoji = status == 'SUCCESS' ? '✅' :
                            status == 'FAILURE' ? '❌' :
                            status == 'ABORTED' ? '🛑' : '⚠️'

                def user = 'N/A'
                try {
                    wrap([$class: 'BuildUser']) {
                        user = env.BUILD_USER
                    }
                } catch (e) {
                    user = 'Unknown'
                }

                def message = "${emoji} *Build ${status}*\n" +
                              "*Job:* ${env.JOB_NAME}\n" +
                              "*Build:* #${env.BUILD_NUMBER}\n" +
                              "*URL:* ${env.BUILD_URL}\n" +
                              "*Started by:* ${user}"

                def jsonMessage = groovy.json.JsonOutput.toJson([text: message])

                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: jsonMessage,
                    url: env.TEAMS_WEBHOOK_URL
                )
            }
        }
    }
}
