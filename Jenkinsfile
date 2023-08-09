// stages:
//   - build
//   - push

// variables:
//   ECR_REGISTRY: $ECR_REGISTRY
//   IMAGE_NAME: backend
//   AWS_REGION: us-east-1
//   AWS_ACCESS_KEY_ID: $AWS_ACCESS_KEY_ID
//   AWS_SECRET_ACCESS_KEY: $AWS_SECRET_ACCESS_KEY

// build:
//   stage: build
//   image: docker:latest
//   services:
//     - docker:dind
//   script:
//     - docker build -t $IMAGE_NAME .
//     - echo "Docker build completed."

// push:
//   stage: push
//   image: python:3.9
//   before_script:
//     - apt-get update
//     - apt-get install -y python3-pip
//     - pip3 install awscli
//   script:
//     - $(aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY)
//     - docker tag $IMAGE_NAME:latest $ECR_REGISTRY/$IMAGE_NAME:latest
//     - docker push $ECR_REGISTRY/$IMAGE_NAME:latest

// after_script:
//   - docker logout $ECR_REGISTRY


pipeline {
    agent any

    environment {
        ECR_REGISTRY = credentials('597993487797') // Add your ECR credentials ID
        IMAGE_NAME = 'backend'
        ECR_REPO = 'backend'          // Replace with your ECR repository name
        AWS_REGION = 'us-east-1'
        AWS_ACCESS_KEY_ID = credentials('AKIAYWOZQPW2TMOJV67V') // Add your AWS Access Key ID credentials ID
        AWS_SECRET_ACCESS_KEY = credentials('KPUoAh5IiZUwpQXRPB3PXKyQ9sNDMz0GsF6DJq+t') // Add your AWS Secret Access Key credentials ID
    }

    stages {
        stage('Build') {
            steps {
                script {
                    def dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}", '.')
                    echo 'Docker build completed.'
                }
            }
        }
        
        stage('Push') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${ECR_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                    sh "docker push ${ECR_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
    
    post {
        always {
            script {
                sh "docker logout ${ECR_REGISTRY}"
            }
        }
    }
}
