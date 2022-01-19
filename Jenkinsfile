pipeline {
  agent { label 'master'}
  options {
    skipDefaultCheckout(true)
  }
    parameters {
    //choice (name: 'awsCredentials' , choice: ['rajrich926'], description: 'the user id to perform the deployment')
    string(name: 'environment', defaultValue: 'default', description: 'Workspace/environment file to use for deployment')
    //choice(name: 'Region' , choice: ['us-east-1'], description: 'the region to deploy to ')
    string(name: 'awsAccountId' , defaultValue: ['595753597126'], description: 'the asw account deploy to ')
    string(name: 'notifyEmail' , defaultValue: ['rajrich926@gmail.com'], description: 'the email address for completion status email to  ')
  }
  environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        TF_IN_AUTOMATION      = '1'
    }

    stages {
        stage ("terraform init") {
            steps {
                sh 'terraform init'
            }
        }
        stage ("terraform fmt") {
            steps {
                sh 'terraform fmt'
            }
        }
        stage ("terraform validate") {
            steps {
                sh 'terraform validate'
            }
        }
        stage ("terrafrom plan") {
            steps {
                sh 'terraform plan '
            }
        }
        stage ("terraform apply") {
            steps {
                sh 'terraform apply --auto-approve'
            }
        }
     }
}
