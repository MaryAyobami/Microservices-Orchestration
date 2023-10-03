pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us east'
        AWS_ACCOUNT_ID = '251126721760'
        ECR_REPO_NAME = 'microservices'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    def dockerComposeFile = 'docker-compose.yml'

                    // Build and push each container image defined in docker-compose.yml
                    sh "docker-compose -f ${dockerComposeFile} build"
                    sh "docker-compose -f ${dockerComposeFile} push"

                    // Authenticate and create the ECR repository (if it doesn't exist)
                    withAWS(credentials: 'your-aws-credentials-id', region: AWS_DEFAULT_REGION) {
                        try {
                            sh "aws ecr create-repository --repository-name ${ECR_REPO_NAME}"
                        } catch (Exception e) {
                            echo "ECR repository already exists."
                        }

                        // Tag and push each image to ECR
                        sh "docker-compose -f ${dockerComposeFile} config --services | xargs -I {} sh -c 'docker tag {}:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO_NAME}:{}'"
                        sh "docker-compose -f ${dockerComposeFile} config --services | xargs -I {} sh -c 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO_NAME}:{}'"
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Cleanup: Remove local Docker images (optional)
                    sh "docker-compose -f ${dockerComposeFile} down --rmi all"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker images pushed to ECR successfully!'
        }
        failure {
            echo 'Failed to push Docker images to ECR.'
        }
    }
}
