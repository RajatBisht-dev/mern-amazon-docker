The Ultimate MERN Stack CI/CD Deployment Guide

This guide breaks down the complete lifecycle of deploying a MERN (MongoDB, Express, React, Node.js) application to an AWS EC2 instance using Docker and GitHub Actions.
Phase 1: Local Setup & Preparation

Before moving to the cloud, the application must run perfectly on your local machine.
1. Clone the Repository

Download the source code to your local machine to begin working.
git clone https://github.com/YourUsername/mern-app.git
2. Install Dependencies

Navigate into both the frontend and backend directories to install the required Node.js packages.
cd backend && npm install
cd ../frontend && npm install
3. Provision the Database

Create a cloud database cluster using MongoDB Atlas. This ensures your data lives in the cloud and isn't tied to your local machine or a single server.
4. Configure Local Environment Variables

Create a .env file in your backend folder. This file securely connects your local code to your new MongoDB database. (Remember to add .env to your .gitignore file!)
Phase 2: Containerization

Packaging the application so it can run identically on any machine, anywhere.
5. Dockerize the Backend

Create a Dockerfile in the backend folder to define the Node.js environment. Pair it with a .dockerignore file (ignoring node_modules) to keep the container lightweight and fast.
6. Dockerize the Frontend

Create a Dockerfile in the frontend folder. This will typically involve a multi-stage build: first using Node to build the React app, and then using Nginx to serve the static files.
7. Configure Nginx (Reverse Proxy)

Create an nginx.conf file. This acts as the "traffic cop" for your application, listening on Port 80 and routing API requests to the backend while serving React pages to the user.
8. Orchestrate with Docker Compose

Create a docker-compose.yml file in the root directory. This master file tells Docker how to build and connect the frontend and backend containers together, mapping the server's Port 80 to the Nginx container's Port 80.
Phase 3: Cloud Infrastructure (AWS)

Setting up the physical server in the cloud that will host the containers.
9. Launch the EC2 Instance

Spin up an Ubuntu EC2 instance on AWS. Attach an Elastic IP to ensure the server's IP address never changes, even if it reboots.
10. Configure the Security Group (Firewall)

By default, AWS blocks all traffic. You must explicitly open the doors to the internet:

    Port 22 (SSH): Allows GitHub Actions to log in and deploy code.

    Port 80 (HTTP): Allows the world to view your website.

11. Set the "Secret Trap" (.env)

SSH into your pristine EC2 server and manually recreate your production .env file inside the target backend folder. This ensures your database passwords are never exposed on GitHub, but are waiting securely for Docker when it boots up.
Phase 4: Automation & CI/CD

Building the robotic pipeline that automatically deploys new code.
12. Secure GitHub Secrets

Create a GitHub repository for your project. Go to Settings > Secrets and variables > Actions and save your AWS credentials so the pipeline can securely log into your server:

    EC2_HOST (Your Elastic IP)

    EC2_USERNAME (ubuntu)

    EC2_SSH_KEY (Your private .pem key)

13. Create the GitHub Actions Workflow

Create a .github/workflows/deploy.yml file. This is the instruction manual for the pipeline. It tells GitHub to:

    Log into the AWS server via SSH.

    Copy the latest code from the main branch.

    Run docker-compose up --build -d to spin up the new containers.

14. The Final Push

Commit your code and push it to the main branch. GitHub Actions will automatically catch the code, transfer it to AWS, and launch your application!
git add .
git commit -m "feat: complete CI/CD pipeline"
git push origin main