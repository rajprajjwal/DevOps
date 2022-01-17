pipeline {
  agent { label 'master'}
  options {
    skipDefaultCheckout(true)
  }
    parameters {
    choice (name: 'awsCredentials' , choice: ['rajrich926'], description: 'the user id to perform the deployment')
    choice (name: 'Region' , choice: ['us-east-1'], description: 'the region to deploy to ')
    string (name: 'awsAccountId' , defaultValue: ['595753597126'], description: 'the asw account deploy to ')
    string (name: 'notifyEmail' , defaultValue: ['rajrich926@gmail.com'], description: 'the email address for completion status email to  ')
  }
  stages{
    stage('clean workspace') {
      steps {
        cleanWs()
      }
    }
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('terraform') {
      steps {
        sh './main.tf apply -auto-approve -no-color'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}
