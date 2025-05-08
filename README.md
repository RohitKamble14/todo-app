To-Do List App
A simple To-Do List application built with HTML, CSS, and JavaScript, dockerized, and deployed on AWS.
Overview
This project demonstrates a single-page To-Do List app with features to add, complete, and delete tasks. It’s dockerized, pushed to DockerHub via GitHub Actions, and deployed on AWS EC2 and EKS.
Features

Add, complete, and delete tasks.
Responsive design with a clean UI.
Dockerized using Nginx.
CI/CD pipeline with GitHub Actions.
Deployed on AWS EC2 and EKS.

Tech Stack

Frontend: HTML, CSS, JavaScript
Containerization: Docker (Nginx)
CI/CD: GitHub Actions
Container Registry: DockerHub
Cloud: AWS (EC2, EKS)

Project Structure
todo-app-eks/
├── index.html               # The main To-Do List app file (HTML, CSS, JS)
├── Dockerfile               # Dockerfile to build the Docker image
├── .github/
│   └── workflows/
│       └── deploy.yml       # GitHub Actions workflow to build and push to DockerHub
├── todo-app-deployment.yaml # Kubernetes Deployment manifest for EKS
├── todo-app-service.yaml    # Kubernetes Service manifest for EKS
└── README.md                # Project overview and documentation

Setup and Installation

Clone the Repository:git clone https://github.com/<your-username>/todo-app-eks.git
cd todo-app-eks


Run Locally:
Use a local server:python3 -m http.server 8080


Visit http://localhost:8080.



Deployment
Part 1: Local Development, Dockerization, and CI/CD Setup
This section covers creating the To-Do List app, dockerizing it, testing it locally, pushing the project to GitHub, and setting up a GitHub Actions workflow to build and push the Docker image to DockerHub.

Create the To-Do List App:

Created index.html, which contains the HTML, CSS, and JavaScript for the To-Do List app.
The app allows users to add tasks, mark them as completed (with a strikethrough), and delete tasks.


Dockerize the App:

Created a Dockerfile to package the app using an Nginx base image:FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]


Built the Docker image locally:docker build -t <your-dockerhub-username>/todo-app:latest .




Test Locally:

Ran the Docker container to test the app:docker run -d -p 8080:80 --name todo-app <your-dockerhub-username>/todo-app:latest


Accessed the app at http://localhost:8080 to verify functionality.
Stopped and removed the container:docker stop todo-app && docker rm todo-app




Push the Image to DockerHub (Manual):

Pushed the image to DockerHub manually to ensure it’s available:docker login
docker push <your-dockerhub-username>/todo-app:latest




Push the Project to GitHub:

Initialized a Git repository:git init
git add index.html Dockerfile README.md
git commit -m "Initial commit: To-Do List app with Dockerfile and README"


Created a public repository on GitHub named todo-app-eks.
Pushed the project:git remote add origin https://github.com/<your-username>/todo-app-eks.git
git branch -M main
git push -u origin main




Set Up GitHub Actions for CI/CD:

Added DockerHub secrets to GitHub:
DOCKERHUB_USERNAME: My DockerHub username.
DOCKERHUB_TOKEN: My DockerHub access token.


Created .github/workflows/deploy.yml to automate building and pushing the Docker image to DockerHub:name: Build and Push to DockerHub
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


Committed and pushed the workflow:git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow to build and push to DockerHub"
git push origin main


The workflow runs on every push to the main branch, building and pushing the image to DockerHub.



Part 2: Deployment on AWS EC2
(To be documented in the next update.)
Part 3: Deployment on AWS EKS
(To be documented in a future update.)
License
MIT License
