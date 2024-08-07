pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id') // Jenkins credentials ID for AWS
        AWS_REGION = 'ap-south-1'
        S3_BUCKET = 'nodejs-artifact-venkat'
        EB_APP_NAME = 'nodejs-jenkins-demo'
        EB_ENV_NAME = 'nodejs-jenkins-demo-env'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Venkat-267/nodejs-jenkins-2'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // You can add test scripts here, e.g., npm test
                echo 'Running tests...'
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r hello-node-app.zip *'
            }
        }

        stage('Upload to S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials-id',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh '''
                    aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                    aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                    aws configure set region $AWS_REGION
                    aws s3 cp hello-node-app.zip s3://$S3_BUCKET/hello-node-app.zip
                    '''
                }
            }
        }

        stage('Deploy to Elastic Beanstalk') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials-id',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh '''
                    aws elasticbeanstalk create-application-version --application-name $EB_APP_NAME --version-label v$BUILD_NUMBER --source-bundle S3Bucket=$S3_BUCKET,S3Key=hello-node-app.zip
                    aws elasticbeanstalk update-environment --environment-name $EB_ENV_NAME --version-label v$BUILD_NUMBER
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
