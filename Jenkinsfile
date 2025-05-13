pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    environment {
        TEAMS_WEBHOOK_URL = 'https://lpnu.webhook.office.com/webhookb2/687e750c-bcee-4590-941e-c594a6d020c9@7631cd62-5187-4e15-8b8e-ef653e366e7a/IncomingWebhook/0f62f9720b8047398e1d80d0a66cb305/ce7527d8-657b-4c4b-a503-21037b76bdf3/V2V6HRLMo5jxqd2JPpdyiC0eH4E3PB0f1ok3UhcEhUj81'
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
        success {
            script {
                def message = """
                ✅ *Build Succeeded*
                *Job:* ${env.JOB_NAME}
                *Build:* #${env.BUILD_NUMBER}
                *URL:* ${env.BUILD_URL}
                """.stripIndent()

                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: """{"text": "${message}"}""",
                    url: env.TEAMS_WEBHOOK_URL
                )
            }
        }

        failure {
            script {
                def message = """
                ❌ *Build Failed*
                *Job:* ${env.JOB_NAME}
                *Build:* #${env.BUILD_NUMBER}
                *URL:* ${env.BUILD_URL}
                """.stripIndent()

                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: """{"text": "${message}"}""",
                    url: env.TEAMS_WEBHOOK_URL
                )
            }
        }
    }
}
