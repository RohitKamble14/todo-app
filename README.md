# To-Do List App

A simple To-Do List application built with HTML, CSS, and JavaScript, dockerized, and deployed on AWS.

## Overview
This project demonstrates a single-page To-Do List app with features to add, complete, and delete tasks. It’s dockerized, pushed to DockerHub via GitHub Actions, and deployed on AWS EC2 and EKS.

## Features
- Add, complete, and delete tasks.
- Responsive design with a clean UI.
- Dockerized using Nginx.
- CI/CD pipeline with GitHub Actions.
- Deployed on AWS EC2 and EKS.

## Tech Stack
- **Frontend**: HTML, CSS, JavaScript
- **Containerization**: Docker (Nginx)
- **CI/CD**: GitHub Actions
- **Container Registry**: DockerHub
- **Cloud**: AWS (EC2, EKS)

## Setup and Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/RohitKamble14/todo-app.git
   cd todo-app
## Dockerize the App
2. **Create a Dockerfile**:
   ```bash
   FROM nginx:alpine 
   COPY index.html /usr/share/nginx/html/index.html 
   EXPOSE 80 CMD ["nginx", "-g", "daemon off;"]
## Test the Docker Image Locally
3. **Build the Docker image**:
   ```bash
   docker build -t todo-app:latest .
4. **Run the container locally to test**:
   ```bash
   docker run -d -p 8081:80 --name todo-app todo-app:latest
5. **Open your browser and go. You should see the To-Do List app.**:
   ```bash
    http://localhost:8081.
6. **Stop and remove the container**:
   ```bash
   docker stop todo-app && docker rm todo-app

## Push the Docker Image to DockerHub
1. **Tag the Image**:
   ```bash
   docker tag todo-app:latest <your-dockerhub-username>/todo-app:latest
2. **Log in to DockerHub from your local machine**:
   ```bash
   docker login
3. **Push the Image**:
   ```bash
   docker push <your-dockerhub-username>/todo-app:latest
4. **Verify the image is available on DockerHub by logging into your DockerHub account and checking your repositories.**:
5. **Set Up a GitHub Repository and Automate Image Building**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: To-Do List app with Dockerfile"
6. **Create a GitHub Repository**:
   ```bash
   Go to GitHub, create a new repository (e.g., todo-app), and push your code:
   git remote add origin https://github.com/<your-username>/todo-app.git
   git branch -M main
   git push -u origin main
7. **Set Up GitHub Secrets**:
   ```bash
   Go to your GitHub repository → Settings → Secrets and variables → Actions → New repository secret.
   Add the following secrets:
   DOCKERHUB_USERNAME: Your DockerHub username.
   DOCKERHUB_TOKEN: Your DockerHub access token (or password).
8. **Create a GitHub Actions Workflow**:
   ```Create a file .github/workflows/deploy.yml in your repository with the following content to automate building and pushing the Docker image to DockerHub
   ```Yaml
   name: Build and Push to DockerHub
   on:
   push:
    branches: [ main ]
   jobs:
   build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Log in to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/todo-app:latest
9. **Commit and push the workflow file**:
    ```bash
    git add .github/workflows/deploy.yml
   git commit -m "Add GitHub Actions workflow to build and push Docker image"
   git push origin main
10. **Verify the Workflow**:
    Go to your GitHub repository → Actions tab → Monitor the workflow run.


   
