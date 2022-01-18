pipeline {
  agent { label 'master'}
  options {
    skipDefaultCheckout(true)
  }
    parameters {
    //choice (name: 'awsCredentials' , choice: ['rajrich926'], description: 'the user id to perform the deployment')
    string(name: 'environment', defaultValue: 'default', description: 'Workspace/environment file to use for deployment')
    choice(name: 'Region' , choice: ['us-east-1'], description: 'the region to deploy to ')
    string(name: 'awsAccountId' , defaultValue: ['595753597126'], description: 'the asw account deploy to ')
    string(name: 'notifyEmail' , defaultValue: ['rajrich926@gmail.com'], description: 'the email address for completion status email to  ')
  }
  environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        TF_IN_AUTOMATION      = '1'
    }

    stages {
        stage('Plan') {
            steps {
                script {
                    currentBuild.displayName = params.version
                }
                sh 'terraform init -input=false'
                sh 'terraform workspace select ${environment}'
                sh "terraform plan -input=false -out tfplan -var 'version=${params.version}' --var-file=environments/${params.environment}.tfvars"
                sh 'terraform show -no-color tfplan > tfplan.txt'
            }
        }

        stage('Approval') {
            when {
                not {
                    equals expected: true, actual: params.autoApprove
                }
            }

            steps {
                script {
                    def plan = readFile 'tfplan.txt'
                    input message: "Do you want to apply the plan?",
                        parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
            }
        }

        stage('Apply') {
            steps {
                sh "terraform apply -input=false tfplan"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'tfplan.txt'
        }
    }
}