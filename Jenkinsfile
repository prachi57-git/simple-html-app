pipeline {
    agent any

    environment {
        S3_BUCKET = 'webpp-code-bucket'
        S3_KEY = 'simple-html-app.zip'
        APPLICATION_NAME = 'WebAppDeploy'
        DEPLOYMENT_GROUP_NAME = 'WebApp-DeploymentGroup'
        AWS_CLI_PROFILE = 'default'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git 'https://github.com/prachi57-git/simple-html-app.git'
            }
        }

        stage('Package') {
            steps {
                script {
                    // Create a zip file of the application directory and files
                    sh 'zip -r simple-html-app.zip public build.sh appspec.yml'
                }
            }
        }

        stage('Upload to S3') {
            steps {
                script {
                    // Upload the zip file to the S3 bucket
                    sh "aws s3 cp simple-html-app.zip s3://webpp-code-bucket/simple-html-app.zip"
                }
            }
        }

        stage('Create Deployment') {
            steps {
                script {
                    // Create a deployment in AWS CodeDeploy
                    sh """
                    aws deploy create-deployment \
                        --application-name WebAppDeploy \
                        --deployment-group-name WebApp-DeploymentGroup \
                        --s3-location bucket=webpp-code-bucket,key=simple-html-app.zip,bundleType=zip
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

