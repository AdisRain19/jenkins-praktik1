pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ **Build SUCCESS** on `${env.BRANCH_NAME}`\n🔗 ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1433028096767955114/1sIkWgSU8fX55p7TnfTlfijyaqJi8x24C-HUvSqY1Mfyl-QDAVxuqdDvuN2_EVlzTZbZ'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ **Build FAILED** on `${env.BRANCH_NAME}`\n🔗 ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1433028096767955114/1sIkWgSU8fX55p7TnfTlfijyaqJi8x24C-HUvSqY1Mfyl-QDAVxuqdDvuN2_EVlzTZbZ'
                )
            }
        }
    }
}
