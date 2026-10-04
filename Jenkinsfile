pipeline {
  agent any

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

    stage('Deploy') {
      steps {
        sh 'cp index.html /deploy/index.html'
        echo "Deployed build ${BUILD_NUMBER}"
      }
    }
  }

  post {
    success { echo 'Pipeline succeeded' }
    failure { echo 'Pipeline failed' }
  }
}
