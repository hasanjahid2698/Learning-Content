# 🐳 Docker Guide

## 📜 Introduction

### Overview of Docker and Containerization

**Docker** is an open-source platform that automates the deployment, scaling, and management of applications using **containerization**.

**Containerization** 📦 is a lightweight form of virtualization that allows you to package an application with all of its dependencies—such as libraries and other configurations—and ship it all out as one single package called a **container**. Unlike a virtual machine, a container runs on the host system's kernel directly, making it much faster and more efficient.

### Benefits of Docker vs. Virtual Machines (VMs)

| Feature          | Docker Containers                             | Virtual Machines                          |
| :--------------- | :-------------------------------------------- | :---------------------------------------- |
| **Startup Time** | Seconds ⚡                                    | Minutes 🐢                                |
| **Resource Usage** | Lightweight (shares host OS kernel)           | Heavyweight (full OS per VM)              |
| **Performance** | Near-native performance                       | Slower due to hardware emulation          |
| **Portability** | Highly portable ("build once, run anywhere")  | Less portable due to OS dependency        |
| **Isolation** | Process-level isolation                       | Full hardware-level isolation             |

---

## ⚙️ Docker Basics

### Key Components

* **Docker Engine**: The core of Docker. It's a client-server application with a daemon process (the `dockerd` command), a REST API, and a command-line interface (CLI) client (`docker` command).
* **Docker CLI**: The primary tool you use to interact with the Docker Engine. You issue commands like `docker run` through the CLI.
* **Docker Desktop**: An easy-to-install application for your Mac or Windows environment that enables you to build and share containerized applications and microservices. It includes the Docker Engine, CLI, and Docker Compose.

### Images and Containers

This is the most fundamental concept in Docker.

* **Image** 🖼️: An image is a **read-only blueprint** or template for creating a container. It contains the application code, libraries, dependencies, and other files needed to run an application. Think of it as a class in object-oriented programming.
* **Container** 📦: A container is a **runnable instance** of an image. You can create, start, stop, move, or delete a container. It's the actual running process. Think of it as an object (an instance of a class).

> **Analogy** 💡: An **image** is like a recipe for a cake. A **container** is the actual cake you baked from that recipe. You can bake many identical cakes (containers) from the same recipe (image).

---

## ⌨️ Basic Commands

Here are the most common commands you'll use daily.

| Command         | Description                                                                          | Example                                                 |
| :-------------- | :----------------------------------------------------------------------------------- | :------------------------------------------------------ |
| `docker run`    | Creates and starts a new container from an image.                                    | `docker run -d -p 8080:80 --name my-nginx nginx`        |
| `docker ps`     | Lists running containers. Add `-a` to see all containers (running and stopped).      | `docker ps -a`                                          |
| `docker stop`   | Gracefully stops one or more running containers.                                     | `docker stop my-nginx`                                  |
| `docker rm`     | Removes one or more stopped containers. Use `-f` to force-remove a running container.| `docker rm my-nginx`                                    |
| `docker pull`   | Downloads an image from a registry (like Docker Hub).                                | `docker pull redis:latest`                              |
| `docker images` | Lists all images stored locally.                                                     | `docker images`                                         |
| `docker exec`   | Executes a command inside a running container. `-it` makes it interactive.           | `docker exec -it my-ubuntu-container /bin/bash`         |
| `docker volume` | Manages volumes for persistent data.                                                 | `docker volume create my-data`                          |

### Running Simple Containers

* **Nginx Web Server**: `docker run -d -p 80:80 --name web-server nginx`
    * `-d`: Detached mode (runs in the background).
    * `-p 80:80`: Maps port 80 of the host to port 80 of the container.
* **Ubuntu**: `docker run -it --name my-ubuntu ubuntu /bin/bash`
    * `-it`: Interactive terminal.
    * `/bin/bash`: The command to run inside the container.

---

## 📝 Dockerfile Basics

A **Dockerfile** is a text file that contains instructions for building a Docker image.

### Writing a Basic Dockerfile

Here's an example for a simple Node.js application:

```dockerfile
# Use an official Node.js runtime as a parent image
FROM node:18-alpine

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install application dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Make port 3000 available to the world outside this container
EXPOSE 3000

# Define the command to run the app
CMD [ "node", "server.js" ]
````

### Building an Image

To build an image from a Dockerfile, use the `docker build` command.

  * **Syntax**: `docker build -t <your-username>/<image-name>:<tag> .`
  * **Example**: `docker build -t my-app:1.0 .`
      * `-t`: Tags the image with a name and optional tag.
      * `.`: Specifies the build context (the current directory).

-----

## ☁️ Docker Hub

**Docker Hub** is a cloud-based registry service where you can find and share container images. It's the default public registry Docker uses.

  * **Pulling Images**: Downloading an image from Docker Hub.
      * `docker pull ubuntu`
  * **Pushing Images**: Uploading your custom-built image to your Docker Hub repository.
    1.  **Tag your image**: `docker tag my-app:1.0 your-dockerhub-username/my-app:1.0`
    2.  **Log in**: `docker login`
    3.  **Push**: `docker push your-dockerhub-username/my-app:1.0`

-----

## 🔄 Container Management

### Starting, Stopping, and Restarting

  * **Start a stopped container**: `docker start <container-id-or-name>`
  * **Stop a running container**: `docker stop <container-id-or-name>`
  * **Restart a container**: `docker restart <container-id-or-name>`

### Inspecting Container Logs

To view the output (logs) from a container, use `docker logs`. This is crucial for debugging.

  * **View logs**: `docker logs <container-id-or-name>`
  * **Follow logs in real-time**: `docker logs -f <container-id-or-name>`

-----

## 💾 Volumes and Persistent Storage

Containers are ephemeral, meaning any data created inside them is lost when the container is removed. **Volumes** are the preferred mechanism for persisting data.

### Using Volumes

  * **Named Volumes**: Docker-managed volumes. This is the recommended approach.
      * Create a volume: `docker volume create my-app-data`
      * Run a container with the volume: `docker run -d -v my-app-data:/path/in/container my-image`
  * **Bind Mounts**: Mounts a file or directory from the host machine into the container. Useful for development.
      * `docker run -d -v /path/on/host:/path/in/container my-image`
      * Using the current directory: `docker run -d -v $(pwd):/app my-image`

-----

## 🌐 Networking

Containers can communicate with each other and the outside world through Docker networks.

### Network Types

  * **`bridge`** (default): Provides internal networking for containers on the same host. Containers can reach each other by name if on a user-defined bridge network.
  * **`host`**: Removes network isolation, letting the container use the host's networking directly.
  * **`none`**: Disables all networking for the container.

### Creating and Using Custom Networks

It's a best practice to create custom bridge networks for your applications.

1.  **Create a network**: `docker network create my-app-network`
2.  **Run containers on that network**:
      * `docker run -d --name db --network my-app-network mongo`
      * `docker run -d --name webapp --network my-app-network -p 8080:3000 my-webapp-image`

Now, the `webapp` container can reach the `db` container using the hostname `db`.

-----

## 🌳 Environment Variables

### Passing Variables

You can pass configuration to containers using environment variables with the `-e` or `--env` flag.

`docker run -d -e DATABASE_URL="mongodb://db:27017/mydb" --network my-app-network my-webapp-image`

### Using `.env` Files

For multiple variables, it's cleaner to use an `--env-file`. In Docker Compose, this is even simpler with a `.env` file.

Create a `.env` file in the same directory as your `docker-compose.yml`:

```env
POSTGRES_USER=myuser
POSTGRES_PASSWORD=mypassword
```

Docker Compose will automatically pick up these variables.

-----

## 🏗️ Dockerizing Applications

This process involves creating a `Dockerfile` for your application and building an image.

### Best Practices for Small and Efficient Images ✨

1.  **Use a small base image**: Start `FROM` a lean base image like `alpine` (e.g., `node:18-alpine`).
2.  **Use a `.dockerignore` file**: Exclude files and directories not needed for the build (like `node_modules`, `.git`, `.env`) to keep the build context small and avoid leaking secrets.
3.  **Leverage layer caching**: Order your Dockerfile instructions from least to most frequently changing. For example, copy `package.json` and run `npm install` *before* you `COPY` your source code.
4.  **Use multi-stage builds**: For compiled languages (like Go, Java, or C\#), use one stage to build the application artifact and a second, smaller stage to run it. This keeps the final image free of build dependencies.

-----

## 🐞 Debugging and Logs

When a container fails or misbehaves, these commands are your best friends.

  * `docker logs <container>`: View standard output from the container. The first place to check.
  * `docker inspect <container>`: Provides detailed, low-level information about the container, including its IP address, volume mounts, and environment variables.
  * `docker exec -it <container> /bin/sh`: Get an interactive shell inside a running container to poke around, check files, and test network connectivity.

### Common Errors

  * **`CrashLoopBackOff` / Exited (1)**: The main process in the container is crashing immediately after starting. Check the logs (`docker logs`) to see the error message.
  * **Connection Refused**: The application inside the container might not be listening on the correct port, or there's a network misconfiguration.

-----

## 🎼 Docker Compose

**Docker Compose** is a tool for defining and running multi-container Docker applications. It uses a YAML file (`docker-compose.yml`) to configure the application's services.

### Introduction to `docker-compose.yml`

Here’s a simple example for a web application with a PostgreSQL database:

```yaml
version: '3.8'

services:
  # Web Application Service
  webapp:
    build: . # Build the image from the Dockerfile in the current directory
    ports:
      - "8000:8000" # Map host port 8000 to container port 8000
    volumes:
      - .:/app # Mount current directory to /app in the container (for development)
    environment:
      - DATABASE_URL=postgres://myuser:mypassword@db:5432/mydatabase
    depends_on:
      - db # Wait for the db service to be ready before starting

  # Database Service
  db:
    image: postgres:14-alpine # Use a pre-built PostgreSQL image
    volumes:
      - db-data:/var/lib/postgresql/data # Persist database data
    environment:
      - POSTGRES_DB=mydatabase
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=mypassword

# Define the named volume
volumes:
  db-data:

```

### Key Commands

  * `docker-compose up`: Builds, (re)creates, starts, and attaches to containers for a service. Add `-d` to run in detached mode.
  * `docker-compose down`: Stops and removes containers, networks, and volumes created by `up`.
  * `docker-compose logs`: Displays log output from services. Use `-f` to follow.

### Multiple Docker Compose Files

You can use multiple Compose files to customize your configuration for different environments (e.g., development vs. production).

  * `docker-compose.yml` (base configuration)
  * `docker-compose.override.yml` (development-specific settings, loaded automatically)
  * `docker-compose.prod.yml` (production-specific settings)

To use a specific file, use the `-f` flag:
`docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d`
