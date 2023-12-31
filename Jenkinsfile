pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east'
        AWS_ACCOUNT_ID = '251126721760'
        ECR_REPO_NAME = 'microservices'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        // stage('Logging into AWS ECR') {
        //     steps {
        //         script {
        //          bat "aws ecr get-login-password - region ${AWS_DEFAULT_REGION} | docker login - username AWS - password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
        //         }
            
        //     }
        // }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    // Build and push each container image defined in docker-compose.yml
                    // bat "docker-compose -f %DOCKER_COMPOSE_FILE% build"
                   // bat "docker-compose -f %DOCKER_COMPOSE_FILE% push"

                    // Authenticate and create the ECR repository (if it doesn't exist)
                    // withAWS(credentials: 'your-aws-credentials-id', region: AWS_DEFAULT_REGION) {
                    //     try {
                    //         bat "aws ecr create-repository --repository-name %ECR_REPO_NAME%"
                    //     } catch (Exception e) {
                    //         echo "ECR repository already exists."
                    //     }

                        // Tag and push each image to ECR
                        // bat "docker-compose -f %DOCKER_COMPOSE_FILE% config --services | ForEach-Object { docker tag \$_:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO_NAME}:\$_ }"
                        // bat "docker-compose -f %DOCKER_COMPOSE_FILE% config --services | ForEach-Object { docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${ECR_REPO_NAME}:\$_ }"
                        // app = docker.build("microservices")
                        docker.withRegistry('http://251126721760.dkr.ecr.us-east-1.amazonaws.com/microservices', 'ecr:us-east-1:251126721760') {
                        // app.push("${env.BUILD_ID}")
                        // app.push("latest")
                         //build image
                        def customImage = docker.build("my-image:${env.BUILD_ID}")

                        //push image
                        customImage.push()

                        }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Cleanup: Remove local Docker images (optional)
                    bat "docker-compose -f %DOCKER_COMPOSE_FILE% down --rmi all"
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