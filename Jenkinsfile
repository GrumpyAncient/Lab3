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
                def buildStatus = currentBuild.currentResult
                def emoji = buildStatus == 'SUCCESS' ? '✅' : (buildStatus == 'FAILURE' ? '❌' : '⚠️')

                def message = """
                ${emoji} *Build ${buildStatus}*
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
