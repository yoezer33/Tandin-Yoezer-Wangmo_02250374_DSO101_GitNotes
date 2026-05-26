# Unit 3: Dockerfile and Docker Compose

## What is a Dockerfile?

A Dockerfile is a plain text file that contains a series of instructions (one per line) used to automatically build a custom Docker image.  
It defines the environment inside the container, including the base operating system, dependencies, application code, and the command to run when the container starts.  
Writing a Dockerfile allows you to version, share, and reproduce your application's environment exactly.

---

## Example Dockerfile (Explained)

Below is a typical Dockerfile for a Node.js application, with comments explaining each instruction:

```dockerfile
# Specify the base image (official Node.js 18 image)
FROM node:18

# Set the working directory inside the container
WORKDIR /app

# Copy package.json first (to leverage Docker caching)
COPY package.json .

# Install dependencies inside the container
RUN npm install

# Copy the rest of the application source code
COPY . .

# Define the command to run when the container starts
CMD ["npm", "start"]
```
## Build an Image
- creates a Docker image named my-app.
```bash
docker build -t my-app .
```

### Docker Compose
- Docker Compose is used to run multiple containers together using a single configuration file.
``` YAML
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"

  app:
    build: .
    ports:
      - "3000:3000"
```
### Docker Compose Commands
``` Bash
docker-compose up       # Start all services
docker-compose down     # Stop all services
docker-compose build    # Build services
```