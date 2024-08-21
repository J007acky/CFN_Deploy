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
                    sh 'sam validate'
                }
            }
        }

        stage('Package SAM Template') {
            steps {
                script {
                    // Package the SAM template, upload artifacts to S3
                    sh "sam package --template-file template.yaml --s3-bucket ${S3_BUCKET} --output-template-file packaged.yaml --region ${AWS_REGION}"
                }
            }
        }

        stage('Deploy SAM Template') {
            steps {
                script {
                    // Deploy the SAM template using CloudFormation
                    sh "sam deploy --template-file packaged.yaml --stack-name ${STACK_NAME} --capabilities CAPABILITY_IAM --region ${AWS_REGION}"
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
