def authorName = ''
def authorEmail = ''

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
          script{
              emailext(
                  subject: "✅ Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                  body: """
                  Hi <strong>${authorName}</strong>,<br><br>
                  The job <strong>${env.JOB_NAME}</strong> (Build #${env.BUILD_NUMBER}) has <strong>succeeded</strong>.<br>
                  <a href="${env.BUILD_URL}">Click here</a> to view the console output.<br><br>
                  Regards,<br>Jenkins
                  """,
                  mimeType: 'text/html',
                  to: authorEmail
            )
        }
        }

        failure {
          script {
            emailext(
                subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Hi <strong>${authorName}</strong>,<br><br>
                The job <strong>${env.JOB_NAME}</strong> (Build #${env.BUILD_NUMBER}) has <strong>failed</strong>.<br>
                <a href="${env.BUILD_URL}">Click here</a> to view the console output.<br><br>
                Regards,<br>Jenkins
                """,
                mimeType: 'text/html',
                to: authorEmail
            )
        }
        }
    }
}

