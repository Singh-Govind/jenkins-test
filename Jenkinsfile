pipeline {
  agent any

  environment {
    APP_ENV     = 'staging'
    APP_VERSION = "1.0.${BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Test') {
      steps { sh 'test -f index.html && echo "index.html exists"' }
    }
    stage('Deploy') {
      steps {
        sh 'sed "s/__ENV__/$APP_ENV/; s/__VERSION__/$APP_VERSION/" index.html > /deploy/index.html'
      }
    }
  }
}
