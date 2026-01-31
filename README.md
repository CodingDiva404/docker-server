# Docker Learning Guide

Docker is a powerful tool for packaging applications into lightweight, portable containers. Think of it like a mini virtual machine (VM), but much lighter and faster. Containers are essentially images of an operating system with your app and its dependencies, making them easy to run anywhere.

---

## What is Docker?

1. Docker is like a VS (Virtual System) but not exactly a full VM — it's much lighter.
2. A Docker image is a snapshot of an OS plus your application and dependencies, which can run consistently on any system.

---

## Steps to Create a Container and Push it to Docker Hub

### 1. Setup

1. Download and install [Docker](https://www.docker.com/get-started) on your system.
2. Create a Docker Hub account at [hub.docker.com](https://hub.docker.com/).

---

### 2. Create a Node.js Server

Start by creating a simple Node.js server:

```bash
npm init -y
npm i express
```

### Create a file index.js and add the following code:

```js
const express = require("express");

const app = express();

app.get('/', (req, res) => {
  res.json({ message: "Hello Docker running 🐳" });
});

app.listen(9000, () => console.log("Server running on PORT 9000"));
```

### 3. Create a Docker Image
#### a) Create a Dockerfile
This file tells Docker how to build your container image:

```docker
# Use the latest Node.js image as a base
FROM node:latest

#Copy all files from current directory to /home/app inside the container
COPY . /home/app

# Set working directory
WORKDIR /home/app/

# Install dependencies
RUN npm install

# Expose the port the app runs on
EXPOSE 9000

# Command to run the app
CMD ["node", "index.js"]
```
### b) Create a .dockerignore file
This file tells Docker which files/folders to ignore while building the image:
```
node_modules/
```
` Tip: Ignoring node_modules prevents unnecessary large files from being added to the image.`

## 4. Build and Run the Docker Container

a) Build the image
``` bash
docker build -t myapp .
```
` The . tells Docker to use the current directory as the build context. `

b) Verify the image
Check Docker Desktop or run:
```bash
docker images
```

c) Run the container
```bash
docker run -p 9000:9000 myapp
```
` This maps port 9000 on your container to port 9000 on your local machine. Open http://localhost:9000 to see the server running. `

## 5. Push the Image to Docker Hub
1. Tag the image (if needed):
   ```bash
   docker -t myapp shraddhad0/mynodeapp
   ```
2. Push the image:
   ```bash
   docker push shraddhad0/mynodeapp
   ```
## 6. Pull and Run Someone Else’s Image
```
docker run -p 8000:8000 piyushgargdev/mynodeapp
```
