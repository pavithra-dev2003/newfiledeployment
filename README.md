# newfiledeployment
code well be added it is used in 
Node.js Sample Application 

This is a simple Node.js application containerized with Docker.
The repository also includes a GitHub Actions workflow that builds the Docker image and pushes it to Docker Hub automatically.

# Features

Node.js 17 app
Dockerized with lightweight Alpine Linux base image
GitHub Actions pipeline for CI/CD
Automatic push to Docker Hub
# Run Locally
1. Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>/nodejs

2. Install dependencies
npm install

3. Run the app
npm start


App will be available at:
👉 http://localhost:3000

Docker
1. Build Docker Image
docker build -t <docker_username>/nodeex:local .

2. Run Container
docker run -p 3000:3000 <docker_username>/nodeex:local

GitHub Actions CI/CD

A GitHub Actions workflow is included at:
.github/workflows/build-docker.yml

Workflow explanation:

Runs on push to main branch

Installs Node.js dependencies

Builds the app

Builds Docker image from nodejs/Dockerfile

Pushes image to Docker Hub

Workflow file:
name: build and push 
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: check out repository
        uses: actions/checkout@v2

      - name: set up node
        uses: actions/setup-node@v2
        with:
          node-version: 17

      - name: install dependency
        run: npm install
        working-directory: nodejs

      - name: login to docker hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: build and push to docker hub
        uses: docker/build-push-action@v2
        with:
          context: nodejs
          file: nodejs/Dockerfile
          push: true
          tags: |
            ${{ secrets.DOCKER_USERNAME }}/nodeex:${{ github.run_number }}

      - name: Docker logout
        run: docker logout

Dockerfile
#Use an official Node.js image as the base image

FROM node:18-alpine 

#Set the working directory in the container

WORKDIR /app

#Copy package.json and package-lock.json files

COPY package.json .

#Install dependencies

RUN npm install

#Copy the rest of the application code

COPY . .

#Expose the application port

EXPOSE 3000

#Run the application

CMD ["npm", "start"]

GitHub Secrets Required

DOCKER_USERNAME → Your Docker Hub username

DOCKER_PASSWORD → Your Docker Hub password / access token

Docker Image Naming

Images will be pushed to Docker Hub as:

<docker_username>/nodeex:<github_run_number>


Example:

pavithra123/nodeex:10
