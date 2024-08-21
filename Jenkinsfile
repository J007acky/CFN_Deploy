pipeline {
  agent any
 
 stages {
        
       stage('Deploy') {
            environment {
                STACK_NAME = 'CfnTask'
                S3_BUCKET = 'rahul-bucket-v2'
                AWS_REGION = 'ap-south-1'
            }
            steps {
                // Deploy the SAM application
                withAWS(credentials: 'jenkins_user', region: "$AWS_REGION") {
                    sh 'sam package --template-file template.yaml --s3-bucket rahul-bucket-v2 --output-template-file output.yaml --debug'
                    sh 'sam deploy --stack-name $STACK_NAME -t output.yaml --s3-bucket $S3_BUCKET --capabilities CAPABILITY_NAMED_IAM'
                }
            }
        }
 }
}
