def authorName = ''
def authorEmail = ''
def ownerEmail = 'rakeshgundla68@gmail.com'

pipeline {
    agent any
    options {
        skipDefaultCheckout()
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Show Commit Author') {
            steps {
                script {
                    authorName = bat(script: '@echo off & "C:\\Djangoprojects\\git\\cmd\\git.exe" log -1 --pretty=format:"%%an"', returnStdout: true).trim()
                    authorEmail = bat(script: '@echo off & "C:\\Djangoprojects\\git\\cmd\\git.exe" log -1 --pretty=format:"%%ae"', returnStdout: true).trim()

                    echo "Author Name: ${authorName}"
                    echo "Author Email: ${authorEmail}"
                }
            }
        }

        stage('Run') {
            steps {
                bat 'python main.py'
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }

        success {
          script {
                def log = currentBuild.rawBuild.getLog(50).join('\n')

                emailext (
                    subject: "✅ SUCCESS: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>Hi <strong>${authorName}</strong>,</p>
                            <p>✅ Build Succeeded for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p>Console Output (last 50 lines):</p>
                            <pre>${log}</pre>
                            <p><a href="${env.BUILD_URL}console">Click here</a> to check full log</p>
                            """,
                    mimeType: 'text/html',
                    to: "${authorEmail}",
                    cc: "${ownerEmail}"
                )
            }
        }

        failure {
          script {
                def log = currentBuild.rawBuild.getLog(50).join('\n')

                emailext (
                    subject: "❌ FAILURE: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>Hi <strong>${authorName}</strong>,</p>
                            <p>❌ Build failed for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p>Console Output (last 50 lines):</p>
                            <pre>${log}</pre>
                            <p><a href="${env.BUILD_URL}console">Click here</a> to check full log</p>
                            """,
                    mimeType: 'text/html',
                    to: "${authorEmail}",
                    cc: "${ownerEmail}"
                )
            }
        }
    }
}

