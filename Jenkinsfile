pipeline {
    agent any

    stages {
        stage("Clone Code from base") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/mandperfect/FullProject_CICD.git", branch: "main"
            }
        }

        //environment {
            //DOCKERHUB_USER = "mandperfect"
            //IMAGE_NAME = "FullProject_CICD"
            //IMAGE_TAG = "2.0"
        //}

        stage("Build") {
            steps {
                echo "Building the Docker image"
                //sh "docker build -t pipeline ."
                //sh "docker compose up -d --build"
                sh "docker build -t mandperfect/pipeline:latest ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag pipeline ${env.dockerHubUser}/pipeline:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/pipeline:latest"
                }
            }
        }

        stage ("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
