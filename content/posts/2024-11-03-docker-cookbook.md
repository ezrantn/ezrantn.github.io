---
title: The Docker Cookbook
date: '2024-11-03'
author: Ezra Natanael
categories:
    - Docker
slug: the-docker-cookbook
toc: true
---

## Introduction

Improving scalability, consistency, and streamlining procedures are critical in today’s fast-paced development environment. Docker, a platform that enables developers to bundle apps and their dependencies into lightweight, portable containers, can help with this. Docker makes it easier to set up environments and deploy apps across several computers, whether you’re working on a personal project or managing an advanced enterprise program.

Software may be executed in isolated settings thanks to Docker containers, so what functions on your computer can function anywhere. This method puts an end to the “it works on my machine” dilemma by putting all the resources your application requires-from system tools to libraries into a single container. Furthermore, the strong Docker ecosystem which includes Docker Compose and Docker Hub - offers orchestration and sharing capabilities, facilitating collaboration and large-scale deployment more easily than before.

From installation to launching your first container, I’ll walk you through the fundamentals of Docker in this tutorial, assisting you in taking the initial steps towards containerization and contemporary application development. This guide might serve as a good starting point for anyone interested in learning how Docker can streamline their workflow, be they a developer, system administrator, or just inquisitive.

## Installation

Installing Docker can feel a bit overwhelming at first, especially if you’re not used to using a terminal. It might even stress you out. But don’t worry! Docker offers a user-friendly interface called Docker Desktop. The best part? Docker Desktop is cross-platform, meaning you can download it on any operating system Windows, Mac, or even Linux.

In this article, I’ll walk you through both methods of creating and running a container, as well as how to configure the environment using both the terminal and Docker Desktop. So, if you’re not familiar with the terminal yet, I’ve got you covered!

### Windows

If you’re using Windows, it’s recommended to install WSL (Windows Subsystem for Linux), a feature of Windows that enables users to run Linux directly on Windows without requiring a separate virtual machine or dual booting.

This integration makes it easier to work with Docker, as it streamlines the process of building, deploying, updating, and shutting down systems using Docker containers.

1. Install Ubuntu from the Microsoft Store 

    Why do we need this? Imagine you’re on your Windows machine but be able to have a Linux terminal. WSL can also manage your Docker containers with improved performance and startup times.

    Here’s the link to download [Ubuntu](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare).

2. Install Docker Desktop for a GUI Experience

   Once Ubuntu is installed, the next step is to install Docker Desktop. You can download it by head over their [website](https://www.docker.com/products/docker-desktop/).

   Docker Desktop is available for multiple platforms!

    > **Note**: Ensure you select the version compatible with your chipset - AMD64 for better software compatibility and performance, or ARM64 for improved power efficiency and lightweight tasks.

3. Create Your Docker Hub Account

   After installing Docker Desktop, it’s recommended to sign up if you don’t already have an account. When you deploy a Docker image, it can be pushed to Docker Hub, with your account name associated with the image, as long as you’re logged in. We’ll explore this process later.

4. Enable WSL 2 in Docker Desktop

   Docker Desktop utilizes WSL 2’s memory management to optimize resource usage, using only the necessary CPU and memory. This enhances performance, especially for resource-intensive tasks like building containers, which is a significant advantage!

   Since we’ve already installed WSL, here’s how to enable WSL 2:
    
    1. Open Docker Desktop and navigate to Settings.
    2. In the General tab, check the box for **Use the WSL 2** based engine. If you installed Docker Desktop on a system that supports WSL 2, this option is enabled by default.
    3. Click Apply & Restart.

   Now, you can run docker commands from Windows using the WSL 2 engine!

5. Test Your Installation

   To ensure everything runs smoothly, we should check the version of Docker we have installed. You can do this easily by entering the following command in the WSL terminal:

    ```shell
    docker version
    ```

   If everything is functioning correctly, you should see output similar to this:

    ```shell
    Client:
    Version:           27.0.3
    API version:       1.46
    Go version:        go1.21.11
    Git commit:        7d4bcd8
    Built:             Sat Jun 29 00:01:25 2024
    OS/Arch:           linux/amd64
    Context:           default
    
    Server: Docker Desktop
    Engine:
    Version:          27.0.3
    API version:      1.46 (minimum version 1.24)
    Go version:       go1.21.11
    Git commit:       662f78c
    Built:            Sat Jun 29 00:02:50 2024
    OS/Arch:          linux/amd64
    Experimental:     false
    containerd:
    Version:          1.7.18
    GitCommit:        ae71819c4f5e67bb4d5ae76a6b735f29cc25774e
    runc:
    Version:          1.7.18
    GitCommit:        v1.1.13-0-g58aa920
    docker-init:
    Version:          0.19.0
    GitCommit:        de40ad0
    ```
   
### MacOS

There are system requirements that vary depending on whether you have an Intel chip or Apple Silicon in your iOS device.

> **Important**! Docker supports Docker Desktop on the most recent versions of macOS. That is, the current release of macOS and the previous two releases. As new major versions of macOS are made generally available, Docker stops supporting the oldest version and supports the newest version of macOS (in addition to the previous two releases).

#### Intel Chip

- A supported version of macOS
- At least 4 GB of RAM.

#### Apple Silicon

- A supported version of macOS.
- At least 4 GB of RAM.
- For the best experience, it is recommended to install Rosetta 2. Although it’s no longer a strict requirement, some optional command line tools still depend on it when using Darwin/AMD64. For more information, see the [known issues](https://docs.docker.com/desktop/troubleshoot/known-issues/). You can install Rosetta 2 manually by running the following command in the terminal:

```shell
softwareupdate --install-rosetta
```

Great! If you’ve confirmed that your Mac is compatible with Docker, let’s go ahead and download it.

1. Download the Installer

   Download the installer by clicking the appropriate download button on this [link](https://www.docker.com/products/docker-desktop/). Be sure to select the version that matches your Mac’s chipset.

2. The Installation Directory

   Double-click **Docker.dmg** to launch the installer, then drag the Docker icon into the **Applications** folder. By default, Docker Desktop will be installed in **/Applications/Docker.app**.

3. Open Up the Docker Application

   Double-click **Docker.app** in the **Applications** folder to start Docker.

4. Review the Docker Subscription Service Agreement in the Docker menu.

5. Click “Accept” to proceed.

6. In the installation window, choose one of the following options:

    a) **Use recommended settings (requires password)**:

    This allows Docker Desktop to automatically configure the necessary settings.

    b) **Use advanced settings**:

   This enables you to customize the following:

    - Location of Docker CLI tools (system or user directory)

    - Enabling the default Docker socket

    - Enabling privileged port mapping

   For more information on these settings, refer to the Docker Desktop [advanced settings documentation](https://docs.docker.com/desktop/settings/#advanced)

7. Click “Finish” to complete the installation.

   If you selected any options requiring a password in step 6, enter your system password when prompted to confirm your choices.

### Linux

While Docker has been available on Linux for years through the command line, Docker Desktop provides a user-friendly graphical interface and additional features that make container management more accessible and efficient.

#### Prerequisites

- A 64-bit version of either Ubuntu, Debian, Fedora, or Red Hat Linux
- KVM virtualization support
- systemd init system
- Gnome or KDE desktop environment
- At least 4GB of RAM

#### Important Notes Before Installation

- **Backup Your Data**: While the installation process is generally safe, it’s always good practice to backup important data before making significant system changes.
- **Previous Docker Installations**: If you have an existing Docker Engine installation, you don’t need to remove it. Docker Desktop can coexist with the existing Docker Engine installation.
- **Root Access**: You’ll need sudo privileges to install Docker Desktop. Ensure your user account has sudo access before proceeding.

If you're already aware of this, let’s go ahead and download it!

1. Update System Package

    First, ensure your system is up to date:

    ```shell
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

2. Install Required Dependencies
 
    ```shell
    sudo apt-get install -y \
     ca-certificates \
     curl \
     gnupg \
     lsb-release
    ```

3. Add Docker’s Official GPG Key

    ```shell
    sudo mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```
   
4. Download Docker Desktop

   Visit the official Docker Desktop page and download the appropriate package for your distribution:

    - For Ubuntu/Debian: Download the **.deb** package
    - For Fedora/Red Hat: Download the **.rpm** package

   Or use wget (example for Ubuntu):
    
    ```shell
    wget https://desktop.docker.com/linux/main/amd64/docker-desktop-4.25.0-amd64.deb
    ```
   
5. Install Docker Desktop

   For Ubuntu/Debian:

    ```shell
    sudo apt-get update
    sudo apt-get install ./docker-desktop-<version>-amd64.deb
    ```
   
    For Fedora/Red Hat:

    ```shell
    sudo dnf install ./docker-desktop-<version>-x86_64.rpm
    ```
   
6. Start Docker Desktop

    You can start Docker Desktop in two ways:

   1. Search for “Docker Desktop” and click on it
   2. From the terminal
   
    ```shell
    systemctl --user start docker-desktop
    ```
   
7. Verify Installation

    Check if Docker Desktop is running properly:

    ```shell
    docker version
    docker compose version
    ```
   
## Docker Images

Docker images are a fundamental concept in Docker, serving as the blueprints for creating Docker containers, which we’ll talk about it later. Understanding Docker images will set you up for success in containerizing applications and managing them efficiently.

### What is Docker Images?

A Docker image is a lightweight, standalone, and executable software package that includes everything needed to run an application. This includes:

- **Code**: The application’s codebase and any executables.
- **Dependencies**: Libraries, binaries, and environment settings needed for the application.
- **System Tools**: Basic tools and utilities required to run the app, often based on a specific OS like Alpine, Debian, or Ubuntu.
  
Docker images are **read-only** and can be shared or used by others, making them highly portable and reusable across different environments.

### Understanding Docker Image Layers

Docker images are built in layers, with each layer representing a filesystem change (like a file addition or command execution). This layering system provides several benefits:

- **Efficiency**: Layers are cached, meaning Docker reuses unchanged layers, speeding up builds.
- **Portability**: Multiple images can share layers, which saves storage and makes images smaller.
- **Version Control**: Each layer corresponds to a step in building the image, so you can revert or modify specific layers without rebuilding the entire image.

### Docker Hub: A Source of Pre-Built Images

Docker Hub is the default public registry for Docker images. It hosts a vast collection of official images for popular software (e.g., Ubuntu, Node.js, PostgreSQL) and community-contributed images.

#### Searching and Pulling an Image

You can search for images on [Docker Hub](https://hub.docker.com/) or directly from the terminal using:

```shell
docker search <image_name>
```

To pull an image, use:

```shell
docker pull <image_name>
```

For example, pulling the official Ubuntu image:

```shell
docker pull ubuntu
```

This command downloads the latest Ubuntu image and saves it locally, making it available for creating containers.

You might be wondering how to build your own Docker image with a customized configuration. The good news is that you can definitely do this using a **Dockerfile**. However, I won’t be covering that in this chapter; instead, I’ll dedicate a separate chapter to the topic. Additionally, I’ll guide you on how to deploy your Docker image effectively.

#### Docker Image Tags

Tags are used to label specific versions of an image, often for version control. When pulling or building an image, the default tag is latest unless specified otherwise.

To pull a specific version of an image, specify the tag:

```shell
docker pull ubuntu:20.04
```

Tags are especially useful for versioning your own images, for example, **my-node-app:1.0**.

#### Managing Docker Images

Docker provides several commands to manage images:

- Listing Images: Use `docker images` to see all local images.
- Removing an Image: To delete an image by name or ID, use:

    ```shell
    docker rmi <image_name_or_id>
    ```

You may need to stop any running containers using the image before deleting it.

In the next section, we’ll explore Docker Containers and how to create and manage them based on the images you’ve learned to build.

## Docker Containers

Docker containers allow applications to run in isolated environments, encapsulating code, dependencies, and configurations into a single package. By building and running containers, you can ensure that your applications behave consistently across various environments, making deployment seamless.

### Key Components

Container building involves creating a Docker image, which acts as a blueprint for your application, packaging all necessary resources. Let’s dive into the essential parts that make up this process:

1. **Dockerfile**

   The **Dockerfile** is a core component of containerization. It’s a simple text file containing instructions that tell Docker how to assemble your application image.

   - Each instruction in a **Dockerfile**, like adding files or setting configurations, creates a “layer” in the image.
   - Layers are cached, meaning Docker can reuse unchanged layers, speeding up the build process. 
   - Using fewer instructions or combining steps where possible reduces the number of layers, optimizing the image size.

   Here is the example of a **Dockerfile** that you might see:

     ```dockerfile
     # Sample Dockerfile
     # Using an official base image
     FROM python:3.9-slim
      
     # Set the working directory in the container
     WORKDIR /app
      
     # Copy the current directory contents into the container at /app
     COPY . /app
      
     # Install any necessary dependencies specified in requirements.txt
     RUN pip install --no-cache-dir -r requirements.txt
      
     # Expose the application on port 5000
     EXPOSE 5000
      
     # Run the application
     CMD ["python", "app.py"]
     ```

     This **Dockerfile** creates a lightweight Python application container:
   
     It starts from the **python:3.9-slim** base image. Sets the working directory, copies files, installs dependencies, and runs the application.

2. **Build Context**

   The build context consists of all the files Docker needs to access while building the image.

   - Typically, it includes the directory containing the **Dockerfile** along with other resources such as source code and configuration files.
   - A `.dockerignore` file can be used to exclude unnecessary files, keeping builds streamlined and secure by excluding sensitive or redundant files.

3. **Base Image**

   The base image acts as the starting layer, providing a foundational operating system for your container.

   - Official images, such as those from Docker Hub, are prebuilt and regularly scanned for security, ensuring a safer starting point.
   - Lightweight images like Alpine reduce size but may lack some essential tools.
   - Distroless images are minimal, providing only essential libraries, which reduces the attack surface and enhances security.

### Build Process Explained

Now, let's walk through creating and running a container using the **Dockerfile** defined earlier.

1. **Build the Docker Image**

   Use the **Dockerfile** to build your Docker image. Open your terminal, navigate to the directory containing your **Dockerfile**, and run:

   ```shell
   docker build -t my-python-app .
   ```

   - `-t my-python-app`: Tags the image as "my-python-app."
   - `.` (dot): Refers to the current directory as the build context, which includes the **Dockerfile** and other necessary files.

2. **Run the Docker Container**

   With the image created, you can start a container using the following command:

   ```shell
   docker run -d -p 5000:5000 --name my-running-app my-python-app
   ```

   - `-d`: Runs the container in detached mode.
   - `-p 5000:5000`: Maps port 5000 on the host to port 5000 on the container.
   - `--name my-running-app`: Names the container.
   - `my-python-app`: Refers to the image you built.

3. **Verify the Container**

   Use `docker ps` to check that the container is running:

   ```shell
   docker ps
   ```

   This will list active containers, showing you details such as the container ID, status, and port mappings.

4. **Stop and Remove the Container**

   When you’re finished, you can stop and remove the container with:

   ```shell
   docker stop my-running-app
   docker rm my-running-app
   ```

### Container Runtime Concepts

Once you have your Docker image, it can be used to create and manage containers. Here are some fundamental concepts of container runtime:

1. **Container Lifecycle**

   Containers progress through different states in their lifecycle:

   - **Created → Running → Paused/Stopped → Removed**
   - Containers can be restarted or stopped, but if explicitly removed, they cannot be restarted.
   - Each container operates within its own isolated environment, enabling multiple instances without conflicts.

2. **Resource Isolation**

   Containers share the host OS kernel but maintain isolated resources.

   - Each container has its own file system, network interfaces, process tree, and can be assigned specific resource limits.
   - This isolation ensures that containers won’t interfere with each other or the host system.

3. **Container Networking**

   Docker provides several networking options:

   - Bridge: Default option that allows container-to-container communication.
   - Host: Directly links the container’s network to the host, providing full access to the host’s network.
   - None: Disables network access, suitable for highly secure setups.
   - Custom: User-defined networks enable grouping and controlling communication between related containers.

### Important Runtime Concepts

1. **Port Mapping**

   Containers are isolated from the host machine by default, but port mapping allows external access.

   - This is done using the `-p host_port:container_port` format.
   - You can map multiple ports for different services within the same container.

2. **Volumes**

   Volumes provide persistent storage, essential for maintaining data across container restarts.

   - Volumes can be shared across containers and come in different types:
     - Named volumes: Managed by Docker and easy to set up.
     - Bind mounts: Map directly to a host folder, giving more control.
     - tmpfs: Temporary storage in memory, ideal for sensitive data that doesn’t need to persist.

3. **Environment Variables**

   These configure how your application behaves without hard-coding values into the codebase.

   - Environment variables can be passed securely to containers and set through:
     - **Dockerfile** ENV instructions,
     - -e flag during runtime,
     - or environment files for managing multiple variables.

### Container Management Concepts

Efficiently managing containers involves understanding their states, allocating resources, and monitoring health:

1. **Container States**

   Containers can exist in various states:

   - Created: Exists but hasn't started running.
   - Running: Actively executing.
   - Paused: Temporarily halted.
   - Stopped: Halted but can be restarted.
   - Deleted: Completely removed

2. **Resource Management**

   Proper resource management ensures that containers don’t exhaust the host’s resources.

   - Set CPU limits to control processing power.
   - Assign memory limits to prevent excessive memory usage.
   - Control disk usage and network bandwidth to manage data consumption.

3. **Health Monitoring**

   Monitoring container health ensures reliable operation:

   - Logs: Track application output for troubleshooting.
   - Stats: Monitor real-time resource usage.
   - Health checks: Regularly assess if the application is running as expected.
   - Events: Track changes in the container’s state, like start, stop, and restart events.

### Best Practices for Container Management

Adhering to best practices can streamline container management and improve reliability:

1. **Container Naming** 

   Using meaningful names helps with organization and identification:

   - Include environment identifiers like “dev” or “prod” in names.
   - Consider automated naming conventions for consistency.

2. **Resource Allocation**

   Managing resources helps avoid overconsumption and enhances performance:

   - Always set memory limits to prevent container crashes.
   - Monitor CPU usage for optimal performance.
   - Use restart policies and implement health checks to ensure stability.

3. **Maintenance**

   Regular maintenance keeps containers up-to-date and secure:

   - Keep images updated to patch vulnerabilities.
   - Set up log rotation to manage storage.
   - Schedule data backups and security scans.

Here is the script to automates some routine container tasks, such as starting, stopping, and removing containers. This helps demonstrate efficient management practices:

```shell
#!/bin/bash
# A script to automate Docker container management

IMAGE_NAME="my-python-app"
CONTAINER_NAME="my-running-app"

# Build the Docker image
echo "Building Docker image..."
docker build -t $IMAGE_NAME .

# Start the container
echo "Starting Docker container..."
docker run -d -p 5000:5000 --name $CONTAINER_NAME $IMAGE_NAME

# Check container status
echo "Checking container status..."
docker ps -f name=$CONTAINER_NAME

# Stop the container
echo "Stopping Docker container..."
docker stop $CONTAINER_NAME

# Remove the container
echo "Removing Docker container..."
docker rm $CONTAINER_NAME
```

Docker containers provide a powerful way to package applications along with all their dependencies, ensuring consistency across different environments. By understanding key concepts like **Dockerfiles**, container lifecycles, resource isolation, and networking, you’re well-equipped to start building and deploying containerized applications.

The next step in containerization involves managing multiple services and components that your application may depend on. For instance, you might need to orchestrate a database, an API, and a web server together. This is where **Docker Compose** comes in. **Docker Compose** lets you define and manage multi-container applications effortlessly, all within a single configuration file.

In the next section, we’ll dive into **Docker Compose**, exploring how to create and manage a multi-container application setup. This will allow you to simulate production environments and set up complex application stacks with ease.

## Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. Using a simple YAML configuration file, you can specify services, networks, and volumes for your app, enabling you to manage everything with a single command.

Docker Compose is perfect for:

- **Microservices architectures**, where each service can be a separate container.
- **Simplified Management**, allowing you to start up or tear down the entire stack with a single command.
- **Development Environments**, enabling easy setup of databases, caches, or other dependencies locally.

### Basic Structure of docker-compose.yml

A basic `docker-compose.yml` file typically includes:

- **Version**: Specifies the Compose file format version.
- **Services**: Lists each container (or service) and its configurations.
- **Networks and Volumes**: Optional but useful for defining network settings and persistent data storage.

### Docker Compose Example for a Web App with PostgreSQL Database

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
      POSTGRES_DB: example_db
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

In this example:

- The `web` service is built from a local Dockerfile and exposed on port 8000.
- The `db` service uses a PostgreSQL image and sets environment variables for database credentials.
- `volumes` are specified to persist the database data outside the container lifecycle.

### Key Docker Compose Commands

- `docker-compose up`: Builds, (re)creates, starts, and attaches to containers.
- `docker-compose down`: Stops and removes containers, networks, and volumes.
- `docker-compose ps`: Lists the running containers.
- `docker-compose logs`: Fetches logs of all services.

### Using Docker Compose in Development and Testing

Docker Compose is invaluable for local development, as it allows you to:

- **Test Multi-Service Configurations** locally.
- **Isolate Development Environments** so each project can have its own dependencies.
- **Easily Scale Services** using `docker-compose up --scale <service>=N`.

Docker Compose makes managing complex applications much simpler by providing a single configuration file to handle multiple services, networking, and volumes. This setup is invaluable for both development and production environments, particularly for microservices.

## Closing

Docker has transformed how we build and deploy applications, making it easier than ever to create consistent, portable, and scalable software. Through this guide, we've explored the fundamentals of containerization, from basic Docker commands to orchestrating multiple services with Docker Compose.

### Key Takeaways

- **Start Simple**: Begin with single containers and gradually move to more complex multi-container applications
- **Think Security**: Always consider security implications when building and deploying containers
- **Optimize Resources**: Keep images small, manage resources effectively, and follow best practices
- **Stay Consistent**: Use Docker Compose to ensure development and production environments remain as similar as possible

### Looking Forward
As you continue your Docker journey, remember that containerization is not just a technology but a mindset. It's about creating reproducible, maintainable, and scalable applications. Whether you're deploying a simple web application or a complex microservices architecture, the principles you've learned here will serve as a solid foundation.

Keep learning, keep experimenting, and most importantly, keep containerizing!

さようなら (Sayōnara),

Ezra