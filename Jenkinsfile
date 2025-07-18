pipeline {
    agent any
    options{
      skipDefaultCheckout()
      timestamps()
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
                    def authorName = bat(script: 'C:\\Djangoprojects\\git\\cmd\\git.exe log -1 --pretty=format:"%%an"', returnStdout: true).trim()
                    def authorEmail = bat(script: 'C:\\Djangoprojects\\git\\cmd\\git.exe log -1 --pretty=format:"%%ae"', returnStdout: true).trim()
                    
                    echo "Author Name: ${authorName}"
                    echo "Author Email: ${authorEmail}"
                }
            }
        }
      stage('Run'){
        steps{
          bat 'python main.py'
        }
      }
    }
}

