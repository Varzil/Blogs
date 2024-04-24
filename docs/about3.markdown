---
layout: page
title: About
permalink: /about3/
---
Name - Varzil Thakkar
Roll No - 21BCP090

## Part 3: Dockerizing the Application

### Step 1: Dockerizing the Backend

Create a Dockerfile for the backend:

```Dockerfile
# Use official Node.js image as the base image
FROM node:14-alpine

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the server application code to container
COPY . .

# Expose port 5000 for the server
EXPOSE 5000

# Command to run the server
CMD ["npm", "start"]
```

### Step 2: Dockerizing the Frontend
Create a Dockerfile for the frontend:
```Dockerfile
# Use official Node.js image as the base image
FROM node:14-alpine as build

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the frontend application code to container
COPY . .

# Build the frontend
RUN npm run build

# Use Nginx as the server to serve the frontend
FROM nginx:alpine

# Copy the built frontend from the build stage to Nginx server
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80 for the frontend
EXPOSE 80

# Command to run the Nginx server
CMD ["nginx", "-g", "daemon off;"]
```
### Step 3: Docker Compose
Create a Docker Compose file named docker-compose.yml in the project root directory:

```yaml
version: '3'

services:
  backend:
    build: ./backend
    ports:
      - '5000:5000'
    environment:
      - MONGODB_URI=mongodb://mongo:27017/todoapp
    depends_on:
      - mongo

  frontend:
    build: ./frontend
    ports:
      - '80:80'

  mongo:
    image: mongo
    ports:
      - '27017:27017'
```
### Step 4: Running the Application with Docker Compose
Run the following command in the project root directory to start the application:

```bash
docker-compose up
```

This command will build and start the containers defined in the docker-compose.yml file, including the backend, frontend, and MongoDB. Access your Todo List application in the web browser at http://localhost.


This part completes the tutorial by dockerizing both the frontend and backend components of the Todo List application and using Docker Compose to orchestrate the containers.
