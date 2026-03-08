pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-2"
        PATH = "/opt/homebrew/bin:${env.PATH}"  // Add AWS CLI path here
    }

    stages {
        stage('Checkout Code') {
            steps { checkout scm }
        }

        stage('Validate Template') {
            steps {
                sh 'aws cloudformation validate-template --template-body file://template.yaml'
            }
        }

        stage('Deploy Stack') {
            steps {
                sh '''
                aws cloudformation deploy \
                --template-file template.yaml \
                --stack-name dev-stack \
                --parameter-overrides InstanceType=t3.micro BucketName=myuniquecloudbucket123 \
                --capabilities CAPABILITY_NAMED_IAM
                '''
            }
        }
    }
}
