pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    environment {
        TEAMS_WEBHOOK_URL = 'https://lpnu.webhook.office.com/webhookb2/687e750c-bcee-4590-941e-c594a6d020c9@7631cd62-5187-4e15-8b8e-ef653e366e7a/IncomingWebhook/bed09bc40eb742f0a2de3d23b91fd631/ce7527d8-657b-4c4b-a503-21037b76bdf3/V2CgRRIzo46kyeqfcQ8Xxc8Zi-t4NoVOfRMQkIPhDYcNc1'
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
