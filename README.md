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
![Screenshot 2025-05-09 103154](https://github.com/user-attachments/assets/722d5b84-7df4-45f4-b2cc-f13a7431428b)

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
    ### Go to your GitHub repository → Actions tab → Monitor the workflow run.

## Deployment on AWS EC2

1. **Log in to the AWS Management Console → EC2 → Launch a new instance**:

2. **Choose Ubuntu Server 22.04 LTS, t2.micro (Free Tier eligible).**:

3. **Create a new key pair or use existing one**:

4. **Configure the security group**:
   ```bash
   Allow SSH (port 22) from your IP (or 0.0.0.0/0 for broader access).
   Allow HTTP (port 80) from 0.0.0.0/0.

5. **Launch the instance**:

6. **SSH into your EC2 instance**:

8. **Install Docker**:
   ```bash
   sudo apt update
   sudo apt install -y docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
   sudo usermod -aG docker ubuntu

9. **Log out and log back in to apply the group changes**:

## Manually Deploy the App on EC2**:

1. **log in to DockerHub first**:
   ```bash
   docker login
2. **Pull the image from DockerHub**:
   ```bash
   docker pull <your-dockerhub-username>/todo-app:latest
3. **Run the Docker Container**:
   ```bash
   docker run -d --name todo-app -p 80:80 <your-dockerhub-username>/todo-app:latest
4. **Verify the Container is Running**:
   ```bash
   docker ps
5. **Access app at http://ec2-public-ip**:
![Screenshot 2025-05-09 111252](https://github.com/user-attachments/assets/6ae8a60e-c61d-4587-8981-4ea198179ca4)


## Deployment on AWS EKS 

1. **Prerequisites (Local Machine)**:
   ```bash
   1. AWS CLI (to interact with AWS services)
   2. aws configure
   2. kubectl (Kubernetes command-line tool)
   3. eksctl (EKS management CLI tool)
2. **Deployment Steps (Create EKS cluster)**:
   ```bash
   eksctl create cluster --name todo-app-cluster --region us-east-1 --nodegroup-name todo-nodes --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3
3. **Configure kubectl**:
   ```bash
   aws eks --region us-east-1 update-kubeconfig --name todo-app-cluster
4. **Create todo-app-deployment.yaml**:
   ```bash
   apiVersion: apps/v1
   kind: Deployment
   metadata:
    name: todo-app-deployment
    labels:
     app: todo-app
   spec:
   replicas: 2
   selector:
    matchLabels:
      app: todo-app
   template:
    metadata:
      labels:
        app: todo-app
    spec:
      containers:
      - name: todo-app
        image: <your-dockerhub-username>/todo-app:latest
        ports:
        - containerPort: 80
5. **Create todo-app-service.yaml**:
   ```bash
   apiVersion: v1
   kind: Service
   metadata:
    name: todo-app-service
   spec:
   selector:
    app: todo-app
   ports:
   - protocol: TCP
    port: 80
    targetPort: 80
   type: LoadBalancer
6. **Deploy to EKS**:
   ```bash
   kubectl apply -f todo-app-deployment.yaml
   kubectl apply -f todo-app-service.yaml
7. **Get LoadBalancer URL**:
   ```bash
   kubectl get services
![Screenshot_9-5-2025_12312_us-east-1 console aws amazon com](https://github.com/user-attachments/assets/b6db6d33-4aa9-4309-a9c0-b8914b07eca0)
![Screenshot 2025-05-09 122831](https://github.com/user-attachments/assets/3b17aaaa-e1cd-475c-a821-2b1703686dfc)

8. **Access app at http://load-balancer-url.**
![Screenshot 2025-05-09 122750](https://github.com/user-attachments/assets/89232525-0283-40ce-b2fa-6a523ff88ed2)


### Cleanup

1. **Delete EKS cluster**:
   ```bash
   eksctl delete cluster --name todo-app-cluster --region us-east-1
2. **Delete Kubernetes resources**:
   ```bash
   kubectl delete -f todo-app-service.yaml
   kubectl delete -f todo-app-deployment.yaml
