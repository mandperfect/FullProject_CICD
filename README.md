# Jenkins Docker Python CI/CD Pipeline

This project demonstrates setting up a Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins and Docker for a Python application.

## Project Structure

- `Dockerfile`: Defines the Docker image for the Python application.
- `Jenkinsfile`: Contains the Jenkins pipeline configuration.
- `docker-compose.yml`: Orchestrates Docker containers for the application and Jenkins.
- `requirements.txt`: Lists the Python dependencies.
- `manage.py`: Entry point for the Python application.
- `pipeline/`: Directory containing application-specific code.

## Prerequisites

- Docker installed on your system.
- Docker Compose installed.
- Basic understanding of Jenkins and Docker.

## Setup Instructions

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/KevDen01/Jenkins-Docker-Python-CI-CD-Pipeline.git
   cd Jenkins-Docker-Python-CI-CD-Pipeline
   ```

2. **Build and Start the Docker Containers:**

   ```bash
   docker-compose up --build
   ```

   This command builds the Docker images and starts the containers as defined in `docker-compose.yml`.

3. **Access Jenkins:**

   - Open your browser and navigate to `http://localhost:8080`.
   - Follow the on-screen instructions to set up Jenkins.

4. **Configure the Jenkins Pipeline:**

   - In Jenkins, create a new Pipeline job.
   - Set the pipeline script to use the `Jenkinsfile` from this repository.

5. **Run the Pipeline:**

   - Trigger the pipeline build in Jenkins.
   - Monitor the build process and ensure all stages complete successfully.

## Application Usage

- **Starting the Application:**

  The application is managed through Docker Compose. Use the following command to start the application:

  ```bash
  docker-compose up
  ```

- **Running Tests:**

  Tests are defined within the pipeline and are executed during the build process.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request.
