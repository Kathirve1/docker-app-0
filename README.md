# docker-app-0

## Building and Deploying a Docker Image to a Linux Server
This guide outlines the steps to build a Docker image and push it to Docker Hub. Once the image is pushed, you can deploy it on a Linux server. This example assumes you have sample application and a Linux server ready for deployment.

### Prerequisites
Before you begin, ensure you have the following:
A GitHub repository containing your application that should be dockerized. A Dockerfile that builds your specific application as a docker image.
Docker Hub account credentials (username and password) stored as GitHub secrets (DOCKER_USERNAME and DOCKER_PASSWORD).

### GitHub Workflow
1. GitHub Workflow Configuration: You should create a GitHub Actions workflow file (e.g., .github/workflows/docker-image.yml) to create a CI/CD pipeline. I have used the below workflow for my pipeline:

```
name: Docker Image Build and Push
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:

  build-push-image:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    - name: Build the Docker image
      run: docker build . --file Dockerfile --tag kathirvel0/docker-app-0:latest

    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
          username: ${{ secrets.DOCKER_USERNAME}}
          password: ${{ secrets.DOCKER_PASSWORD }}
          
    - name: Push Docker image to Docker Hub
      run: docker push kathirvel0/docker-app-0:latest
```
2. Workflow Explanation:
The provided GitHub Actions workflow is named "Docker Image Build and Push." It automates the process of building a Docker image from your code, logging in to Docker Hub, and pushing the image to a Docker Hub repository.

Workflow Triggers:-
* on: Specifies when the workflow should be triggered.
* push: The workflow will run when code is pushed to the repository.
* branches: [ "main" ]: This workflow will only be triggered when changes are pushed to the "main" branch.
* pull_request: The workflow will also run when pull requests are opened or updated on the "main" branch

Workflow Jobs:-
* build-push-image: This is the only job defined in this workflow.
* runs-on: ubuntu-latest: Specifies that this job will run on a GitHub-hosted runner using the latest version of Ubuntu.
* steps: Defines a series of steps to be executed in this job.
* uses: actions/checkout@v3: This step checks out the code from the repository onto the runner, allowing subsequent steps to access your project's files.
* name: Build the Docker image: This step gives a name to the action that follows. It's for clarity in the workflow log.
* run: docker build . --file Dockerfile --tag kathirvel0/docker-app-0:latest : This command builds a Docker image from your project's code. It uses the Dockerfile located in the project directory and tags the image as kathirvel0/docker-app-0:latest.
* name: Login to Docker Hub: This step is used to log in to Docker Hub using your Docker Hub credentials. It prepares the runner to push the image to Docker Hub.
* uses: docker/login-action@v2: This action is used to authenticate with Docker Hub.
* with: Specifies input parameters for the action. The username and password followed by this line will be taken as input parameters to login to the Docker hub.
* username: ${{ secrets.DOCKER_USERNAME}}: The Docker Hub username is retrieved from a GitHub secret named DOCKER_USERNAME.
* password: ${{ secrets.DOCKER_PASSWORD }}: The Docker Hub password is retrieved from a GitHub secret named DOCKER_PASSWORD.
* name: Push Docker image to Docker Hub: This step pushes the previously built Docker image to a Docker Hub repository.
* run: docker push kathirvel0/docker-app-0:latest : This command pushes the image to the Docker Hub repository named kathirvel0/docker-app-0 with the latest tag.


### Deployment on Linux Server:
* On your Linux server, ensure Docker is installed and is on active status.
* Pull the Docker image from Docker Hub:

```
docker pull kathirvel0/docker-app-0:latest
```
* Run your application using the pulled image:

```
docker run -d -p 8080:80 kathirvel0/docker-app-0:latest
```
* Your Dockerized application should now be running on port 8080 on your Linux server.

### Access the Application:
Open a web browser and access your application by entering the server's IP address or domain name and port 8080 (e.g., http://your-server-ip:8080).

![image](https://github.com/Kathirve1/docker-app-0/assets/109842217/a5ebe457-5b1c-485d-ba9b-dc458228aab4)
