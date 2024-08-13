pipeline {
  agent any
 
 stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/J007acky/CFN_Deploy.git'
            }
          
        }
       stage('Deploy') {
            environment {
                STACK_NAME = 'CfnTask'
                S3_BUCKET = 'my-s3-bucket'
                AWS_REGION = 'ap-south-1'
            }
            steps {
                // Deploy the SAM application
                withAWS(credentials: 'jenkins_user', region: "$AWS_REGION") {
                    sh 'sam deploy --stack-name $STACK_NAME -t template.yaml --s3-bucket $S3_BUCKET --capabilities CAPABILITY_IAM'
                }
            }
        }
 }
}
