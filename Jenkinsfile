pipeline {
  agent { label 'linux-agent' }

  environment {
    APP_VERSION = "1.0.${BUILD_NUMBER}"
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

    stage('Deploy to Staging') {
      steps {
        sh '''
          mkdir -p /deploy/staging
          sed "s/__ENV__/staging/; s/__VERSION__/$APP_VERSION/" index.html > /deploy/staging/index.html
        '''
      }
    }

    stage('Smoke test Staging') {
      steps {
        sh 'grep -q "Environment: staging" /deploy/staging/index.html && echo "Staging looks good"'
      }
    }

    stage('Approval') {
      steps {
        timeout(time: 10, unit: 'MINUTES') {
          input message: "Deploy ${APP_VERSION} to production?", ok: 'Deploy to prod'
        }
      }
    }

    stage('Deploy to Production') {
      steps {
        sh '''
          mkdir -p /deploy/prod
          sed "s/__ENV__/prod/; s/__VERSION__/$APP_VERSION/" index.html > /deploy/prod/index.html
        '''
      }
    }
  }

  post {
    success { echo "Released ${APP_VERSION} to production" }
    failure { echo 'Pipeline failed' }
    aborted { echo 'Production deploy was not approved' }
  }
}
