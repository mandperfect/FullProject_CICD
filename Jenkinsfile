pipeline {
    agent any

    environment {
            DOCKERHUB_USER = "mandperfect"
            IMAGE_NAME = "fullpipeline"
            IMAGE_TAG = "2.0"
        }


    stages {
        stage("Clone Code from base") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/mandperfect/FullProject_CICD.git", branch: "main"
            }
        }

     stages {
        stage('SonarQube Scan (SAST)') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    echo "Running SonarQube scan"
                    sh """
                        docker run --rm \
                          -v \$(pwd):/usr/src \
                          -e SONAR_HOST_URL=${SONAR_HOST_URL} \
                          -e SONAR_LOGIN=${SONAR_TOKEN} \
                          sonarsource/sonar-scanner-cli \
                          -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                          -Dsonar.sources=. \
                          -Dsonar.inclusions=Dockerfile,**/*.py \
                          -Dsonar.exclusions=**/node_modules/**,**/target/**
                    """
                }
            }
        }

        stage('Sonar Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
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
    

        stage ("Deploy") {
            steps {
                echo "Deploying the container"
               // sh "docker compose down && docker compose up -d"
            }
        }
    }
}
