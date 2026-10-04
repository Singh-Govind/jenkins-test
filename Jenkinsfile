pipeline {
  agent any

  environment {
    APP_ENV     = 'staging'
    APP_VERSION = "1.0.${BUILD_NUMBER}"
    API_KEY     = credentials('api-key')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Test') {
      steps {
        sh 'test -f index.html && echo "index.html exists"'
      }
    }

    stage('Secret check') {
      steps {
        sh 'echo "Key is: $API_KEY"'
      }
    }

    stage('Deploy') {
      steps {
        sh 'sed "s/__ENV__/$APP_ENV/; s/__VERSION__/$APP_VERSION/" index.html > /deploy/index.html'
      }
    }
  }

  post {
    success { echo 'Pipeline succeeded' }
    failure { echo 'Pipeline failed' }
  }
}
