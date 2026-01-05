Dockerized Application with GitHub Actions CI/CD:
This repository demonstrates a Docker-based application with an automated GitHub Actions CI/CD pipeline for building, testing, and optionally pushing Docker images.

Features:
🐳 Dockerized application
🤖 GitHub Actions for CI/CD
🔁 Automated build on push & pull request
🔐 Secure secrets handling
📦 Optional image push to Docker Hub / Container Registry

🧱 Project Structure:
├── .github/
│ └── workflows/
│ └── docker-ci.yml
├── Dockerfile
├── .dockerignore
├── src/
│ └── app
├── README.md

🐳 Docker:
Dockerfile (example)
FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update &&  \
    apt-get install -y nginx && \
    rm -rf /var/lib/apt/lists/*
COPY index.html /var/www/html/index.html
EXPOSE 80 
CMD ["nginx","-g","daemon off;"]

Build Docker Image Locally
docker build -t myapp:latest .

Run Container Locally
docker run -p 3000:3000 myapp:latest

🤖 GitHub Actions CI/CD
The workflow automatically:
Checks out the code
Logs into Docker registry (optional)
Builds the Docker image
Pushes the image (optional)

Workflow File
.github/workflows/docker-ci.yml
name: build and push docker image from the github action  

on:
  pull_request:
    branches:
        - main 
  push: 
    branches:
        - main 
    
  workflow_dispatch:
 
jobs:
    publish_image:
        runs-on: ubuntu-latest 
        
        steps: 
        - name: checkout code 
          uses: actions/checkout@v4 
        - name: Docker image build 
          run: |
                docker build -t ${{ secrets.DOCKER_HUB_USERNAME}}/docker-project-image-push:main .
        - name: Docker push images to dockerhub 
          run: | 
               docker login -u ${{ secrets.DOCKER_HUB_USERNAME}} -p ${{ secrets.DOCKER_HUB_TOKEN}}
               docker push ${{ secrets.DOCKER_HUB_USERNAME}}/docker-project-image-push:main 

🔐 GitHub Secrets Configuration
Go to: Repository → Settings → Secrets and variables → Actions
Add the following secrets:
Name	Description
DOCKER_USERNAME	Docker Hub username
DOCKER_PASSWORD	Docker Hub password or access token

▶️ Running the Pipeline
Push code to main branch → pipeline runs automatically
Pull requests trigger build-only checks

You can view results under: GitHub → Actions tab
