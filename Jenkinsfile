pipeline {
    agent any

    environment {
            DOCKERHUB_USER = "mandperfect"
            IMAGE_NAME = "FullProject_CICD"
            IMAGE_TAG = "2.0"
        }


    stages {
        stage("Clone Code from base") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/mandperfect/FullProject_CICD.git", branch: "main"
            }
        }

        environment {
            DOCKERHUB_USER = "mandperfect"
            IMAGE_NAME = "FullProject_CICD"
            IMAGE_TAG = "2.0"
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t mandperfect/pipeline:latest ."
            }
        }

         stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                sh """
                    echo "Pushing image..."
                    docker push $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                """
            }
        }
    }

        stage ("Deploy") {
            steps {
                echo "Deploying the container"
               // sh "docker compose down && docker compose up -d"
            }
        }
}
