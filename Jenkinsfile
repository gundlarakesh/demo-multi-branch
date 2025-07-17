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
      stage('Run'){
        steps{
          bat 'python main.py'
        }
      }
    }
}
