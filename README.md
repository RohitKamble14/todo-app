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
   FROM nginx:alpine COPY index.html /usr/share/nginx/html/index.html EXPOSE 80 CMD ["nginx", "-g", "daemon off;"]

## Test the Docker Image Locally
