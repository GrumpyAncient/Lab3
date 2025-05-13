import groovy.json.JsonOutput

post {
    success {
        script {
            def message = """✅ *Build Succeeded*
*Job:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*URL:* ${env.BUILD_URL}"""

            def payload = JsonOutput.toJson([text: message])

            httpRequest(
                httpMode: 'POST',
                contentType: 'APPLICATION_JSON',
                requestBody: payload,
                url: env.TEAMS_WEBHOOK_URL
            )
        }
    }

    failure {
        script {
            def message = """❌ *Build Failed*
*Job:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*URL:* ${env.BUILD_URL}"""

            def payload = JsonOutput.toJson([text: message])

            httpRequest(
                httpMode: 'POST',
                contentType: 'APPLICATION_JSON',
                requestBody: payload,
                url: env.TEAMS_WEBHOOK_URL
            )
        }
    }
}
