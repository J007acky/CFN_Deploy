pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1' // Set your default AWS region
        S3_BUCKET = 'rahul-bucket-v2' // S3 bucket to store packaged template
        STACK_NAME = 'SAMjenkins' // CloudFormation stack name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install SAM CLI') {
            steps {
                script {
                    // Install AWS SAM CLI if not installed
                    sh 'if ! command -v sam &> /dev/null; then pip install aws-sam-cli; fi'
                }
            }
        }

        stage('Validate SAM Template') {
            steps {
                script {
                    // Validate the SAM template
                    sh 'sam validate --region ${AWS_REGION}'
                }
            }
        }

        stage('Deploying Template A') {
            steps {
                script {
                    withAWS(credentials: 'aws-access', region: "$AWS_REGION") {
                    // Package the SAM template, upload artifacts to S3
                    sh "sam package --template-file A.yaml --s3-bucket ${S3_BUCKET} --output-template-file packagedA.yaml --region ${AWS_REGION}"
                    sh "sam deploy --template-file packagedA.yaml --stack-name ${STACK_NAME} --capabilities CAPABILITY_IAM --region ${AWS_REGION}"
                }
                }
            }
        }

        stage('Deploying Template B') {
            steps {
                script {
                    withAWS(credentials: 'aws-access', region: "$AWS_REGION") {
                    // Deploy the SAM template using CloudFormation
                    sh "sam package --template-file B.yaml --s3-bucket ${S3_BUCKET} --output-template-file packagedA.yaml --region ${AWS_REGION}"
                    sh "sam deploy --template-file packagedB.yaml --stack-name ${STACK_NAME} --capabilities CAPABILITY_IAM --region ${AWS_REGION}"
                }
                }
            }
        }

        stage('Clean Up') {
            steps {
                cleanWs()
            }
        }
    }

    post {
        always {
            echo 'Deployment finished.'
        }
    }
}
