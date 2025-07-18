pipeline {
    agent any
    options{
      skipDefaultCheckout()
      timestamps()
    }

    environment {
        AUTHOR_NAME = ''
        AUTHOR_EMAIL = ''
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        stage('Show Commit Author') {
            steps {
                script {
                    env.AUTHOR_NAME = bat(script: '@echo off & "C:\\Djangoprojects\\git\\cmd\\git.exe" log -1 --pretty=format:"%%an"', returnStdout: true).trim()
                    env.AUTHOR_EMAIL = bat(script: '@echo off & "C:\\Djangoprojects\\git\\cmd\\git.exe" log -1 --pretty=format:"%%ae"', returnStdout: true).trim()

                    echo "Author Name: ${env.AUTHOR_NAME}"
                    echo "Author Email: ${env.AUTHOR_EMAIL}"
                }
            }
        }
      stage('Run'){
        steps{
          bat 'python main.py'
        }
      }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            // send email notification to the author
            emailext(
                subject: "Build Successful",
                body: """
                Hi <strong>${env.AUTHOR_NAME}</strong>,
                <p>The ${env.JOB_NAME} with build number ${env.BUILD_NUMBER} has <strong>succeeded.</strong></p>
                <p>Check the console output at <a href="${env.BUILD_URL}">this link</a>.</p>
                """,
                to: env.AUTHOR_EMAIL
            )
        }
        failure {
          // include the build information in the email
            emailext(
                subject: "Build Failed",
                body: """
                Hi <strong>${env.AUTHOR_NAME}</strong>,
                <p>The ${env.JOB_NAME} with build number ${env.BUILD_NUMBER} has <strong>failed.</strong></p>
                <p>Check the console output at <a href="${env.BUILD_URL}">this link</a>.</p>
                """,
                to: env.AUTHOR_EMAIL
            )
        }
    }
}

