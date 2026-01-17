pipeline {
    agent any

    environment {
            DOCKERHUB_USER = "mandperfect"
            IMAGE_NAME = "fullpipeline"
            IMAGE_TAG = "2.0"
            AWS_REGION = "ap-south-1"
            
        }


    stages {
        stage("Clone Code from base") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/mandperfect/FullProject_CICD.git", branch: "main"
            }
        }
        
    
        stage("Build") {
            steps {
                echo "Building the Docker image"
               // sh "docker build -t pipeline ."
                sh "docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG ."
            }
        }

          stage("Scan Image with Trivy") {
                steps {
                    echo "Scanning Docker image for HIGH and CRITICAL vulnerabilities"
                    sh '''
                        docker run --rm \
                          -v /var/run/docker.sock:/var/run/docker.sock \
                          aquasec/trivy:latest image \
                          ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
        }

         stage('Login to AWS ECR') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    aws ecr get-login-password --region ${AWS_REGION} \
                    | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    '''
                }
            }
        }

     stage('Push Image to ECR') {
            steps {
                sh '''
                docker push ${{AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${IMAGE_NAME}}:${IMAGE_TAG}
                
                '''
            }
        }

        stage ("Deploy") {
            steps {
                echo "Deploying the container"
               // sh "docker compose down && docker compose up -d"
            }
        }
    }
}
