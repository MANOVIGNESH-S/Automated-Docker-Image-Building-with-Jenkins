Automated Docker Image Building with Jenkins

Overview

This project demonstrates how to automate the process of building Docker images using Jenkins. It sets up a Jenkins pipeline to pull the latest source code, build a Docker image, and push it to a container registry.

Prerequisites

Before setting up the Jenkins pipeline, ensure you have the following installed and configured:

Jenkins (with Docker and Pipeline plugins)

Docker (installed on the Jenkins server)

GitHub/GitLab Repository (containing the Dockerfile and source code)

Container Registry (Docker Hub, AWS ECR, or any other registry)

Setup Instructions

1. Install Jenkins and Required Plugins

Install Jenkins and the following plugins:

Pipeline

Docker Pipeline

Git

2. Configure Jenkins to Use Docker

Ensure the Jenkins user has permission to run Docker:

sudo usermod -aG docker jenkins

Restart Jenkins after adding the user.

3. Create a Jenkins Pipeline

Navigate to Jenkins Dashboard → New Item → Pipeline.

Add a pipeline script in the project.

Configure credentials for accessing the container registry (if required).

4. Define the Jenkinsfile

Create a Jenkinsfile in the project repository:

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-docker-image-name'
        DOCKER_REGISTRY = 'your-container-registry'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE:latest .'
                }
            }
        }

        stage('Push to Registry') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-credentials', url: "https://$DOCKER_REGISTRY"]) {
                        sh 'docker push $DOCKER_IMAGE:latest'
                    }
                }
            }
        }
    }
}

Running the Pipeline

Push the Jenkinsfile to your repository.

Trigger a Build in Jenkins.

The pipeline will checkout the latest code, build a Docker image, and push it to the registry.

Conclusion

This setup enables continuous integration and deployment for Docker-based applications, making it easier to automate builds and deployments. Happy coding!
