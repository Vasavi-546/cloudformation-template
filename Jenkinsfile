pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-2'
    }

    stages {

        stage('Validate CloudFormation Template') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    /opt/homebrew/bin/aws cloudformation validate-template \
                    --region $AWS_DEFAULT_REGION \
                    --template-body file://template.yaml
                    '''
                }
            }
        }

        stage('Deploy Stack') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    /opt/homebrew/bin/aws cloudformation deploy \
                    --region $AWS_DEFAULT_REGION \
                    --template-file template.yaml \
                    --stack-name my-cloudformation-stack \
                    --parameter-overrides BucketName=my-jenkins-pipeline-bucket \
                    --capabilities CAPABILITY_NAMED_IAM
                    '''
                }
            }
        }

    }
}
