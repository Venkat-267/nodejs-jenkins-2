pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id') // Jenkins credentials ID for AWS
        AWS_REGION = 'ap-south-1'
        S3_BUCKET = 'nodejs-artifact-venkat'
        EB_APP_NAME = 'nodejs-jenkins-demo
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
                withAWS(region: "${AWS_REGION}", credentials: "${AWS_CREDENTIALS}") {
                    s3Upload(bucket: "${S3_BUCKET}", file: 'hello-node-app.zip', path: "hello-node-app.zip")
                }
            }
        }

        stage('Deploy to Elastic Beanstalk') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: "${AWS_CREDENTIALS}") {
                    elasticBeanstalkDeploy(
                        applicationName: "${EB_APP_NAME}",
                        environmentName: "${EB_ENV_NAME}",
                        s3Bucket: "${S3_BUCKET}",
                        s3Key: "hello-node-app.zip",
                        versionLabel: "v${env.BUILD_NUMBER}"
                    )
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
