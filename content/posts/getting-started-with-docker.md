---
title: "Getting Started with Docker"
date: "2024-09-07"
summary: "Introduction to Docker"
description: "Introduction to Docker"
toc: true
readTime: true
autonumber: true
math: true
tags: ["docker", "tutorial"]
showTags: false
hideBackToTop: false
---

## Introduction

Improving scalability, consistency, and streamlining procedures are critical in today's fast-paced development environment. Docker, a platform that enables developers to bundle apps and their dependencies into lightweight, portable containers, can help with this. Docker makes it easier to set up environments and deploy apps across several computers, whether you're working on a personal project or managing an advanced enterprise program.

Software may be executed in isolated settings thanks to Docker containers, so what functions on your computer can function anywhere. This method puts an end to the "it works on my machine" dilemma by putting all the resources your application requires-from system tools to libraries into a single container. Furthermore, the strong Docker ecosystem which includes Docker Compose and Docker Hub—offers orchestration and sharing capabilities, facilitating collaboration and large-scale deployment more easily than before.

From installation to launching your first container, I'll walk you through the fundamentals of Docker in this tutorial, assisting you in taking the initial steps towards containerization and contemporary application development. This guide might serve as a good starting point for anyone interested in learning how Docker can streamline their workflow, be they a developer, system administrator, or just inquisitive.

## Installation

Installing Docker can feel a bit overwhelming at first, especially if you're not used to using a terminal. It might even stress you out. But don't worry! Docker offers a user friendly interface called Docker Desktop. The best part? Docker Desktop is cross-platform, meaning you can download it on any operating system Windows, Mac, or even Linux.

In this article, I’ll walk you through both methods of creating and running a container, as well as how to configure the environment using both the terminal and Docker Desktop. So, if you're not familiar with the terminal yet, I've got you covered!

### Windows

If you're using Windows, it's recommended to install WSL (Windows Subsystem for Linux), a feature of Windows that enables users to run Linux directly on Windows without requiring a separate virtual machine or dual booting.

This integration makes it easier to work with Docker, as it streamlines the process of building, deploying, updating, and shutting down systems using Docker containers.

1. Install Ubuntu from the Microsoft Store

    Why do we need this? Imagine you're on your Windows machine but be able to have a Linux terminal. WSL can also manage your Docker containers with improved performance and startup times.

    Here's the link to download [Ubuntu](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare).

2. Install Docker Desktop for a GUI Experience

   Once Ubuntu is installed, the next step is to install Docker Desktop. You can download it by clicking  [this link](https://www.docker.com/products/docker-desktop/).

   Docker Desktop is available for multiple platforms!

   >> **Note:** Ensure you select the version compatible with your chipset—AMD64 for better software compatibility and performance, or ARM64 for improved power efficiency and lightweight tasks.

3. Create Your Docker Hub Account

    After installing Docker Desktop, it’s recommended to sign up if you don’t already have an account. When you deploy a Docker image, it can be pushed to Docker Hub, with your account name associated with the image, as long as you're logged in. We’ll explore this process later.

4. Enable WSL 2 in Docker Desktop

    Docker Desktop utilizes WSL 2's memory management to optimize resource usage, using only the necessary CPU and memory. This enhances performance, especially for resource-intensive tasks like building containers, which is a significant advantage!

    Since we've already installed WSL, here’s how to enable WSL 2:

    1. Open Docker Desktop and navigate to Settings.
    2. In the General tab, check the box for **Use the WSL 2 based engine**. If you installed Docker Desktop on a system that supports WSL 2, this option is enabled by default.
    3. Click Apply & Restart.

   Now, you can run `docker` commands from Windows using the WSL 2 engine!

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

>> **Important!** Docker supports Docker Desktop on the most recent versions of macOS. That is, the current release of macOS and the previous two releases. As new major versions of macOS are made generally available, Docker stops supporting the oldest version and supports the newest version of macOS (in addition to the previous two releases).

#### Intel Chip

- A supported version of macOS.
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

    Download the installer by clicking the appropriate download button on [this link](https://www.docker.com/products/docker-desktop/). Be sure to select the version that matches your Mac's chipset.

2. The Installation Directory

   Double-click `Docker.dmg` to launch the installer, then drag the Docker icon into the **Applications** folder. By default,
   Docker Desktop will be installed in `/Applications/Docker.app`.

3. Open Up the Docker Application

   Double-click `Docker.app` in the **Applications** folder to start Docker.

4. Review the Docker Subscription Service Agreement in the Docker menu.

5. Click "Accept" to proceed.

6. In the installation window, choose one of the following options:

    **a) Use recommended settings (requires password):**

    This allows Docker Desktop to automatically configure the necessary settings.

    **b) Use advanced settings:**

    This enables you to customize the following:

    - Location of Docker CLI tools (system or user directory)

    - Enabling the default Docker socket

    - Enabling privileged port mapping

    For more information on these settings, refer to the Docker Desktop [documentation](https://docs.docker.com/desktop/settings/#advanced).

7. Click "Finish" to complete the installation.

   If you selected any options requiring a password in step 6, enter your system password when prompted to confirm your choices.

### Linux

While Docker has been available on Linux for years through the command line, Docker Desktop provides a user-friendly graphical interface and additional features that make container management more accessible and efficient.

**Prerequisites**:

- A 64-bit version of either Ubuntu, Debian, Fedora, or Red Hat Linux
- KVM virtualization support
- systemd init system
- Gnome or KDE desktop environment
- At least 4GB of RAM

**Important Notes Before Installation**:

- **Backup Your Data**: While the installation process is generally safe, it's always good practice to backup important data before making significant system changes.
- **Previous Docker Installations**: If you have an existing Docker Engine installation, you don't need to remove it. Docker Desktop can coexist with the existing Docker Engine installation.
- **Root Access**: You'll need sudo privileges to install Docker Desktop. Ensure your user account has sudo access before proceeding.

1. Update System Packages

    First, ensure your system is up to date:

    ```bash
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

2. Install Required Dependencies

   Install necessary dependencies:

   ```bash
   sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
   ```

3. Add Docker's Official GPG Key

   ```bash
   sudo mkdir -m 0755 -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   ```

4. Download Docker Desktop

   Visit the official Docker Desktop page and download the appropriate package for your distribution:

   - For Ubuntu/Debian: Download the `.deb` package
   - For Fedora/Red Hat: Download the `.rpm` package

   Or use wget (example for Ubuntu):

   ```bash
   wget https://desktop.docker.com/linux/main/amd64/docker-desktop-4.25.0-amd64.deb
   ```

5. Install Docker Desktop

   For Ubuntu/Debian:

   ```bash
   sudo apt-get update
   sudo apt-get install ./docker-desktop-<version>-amd64.deb
   ```

   For Fedora/Red Hat:

   ```bash
   sudo dnf install ./docker-desktop-<version>-x86_64.rpm
   ```

6. Start Docker Desktop

   You can start Docker Desktop in two ways:

   1. Search for **"Docker Desktop"** and click on it
   2. From the terminal

   ```bash
   systemctl --user start docker-desktop
   ```

7. Verify Installation

   Check if Docker Desktop is running properly:

   ```bash
   docker version
   docker compose version
   ```

## Creating a Container

### What is a Container?

A container is like a lightweight, standalone package that contains everything needed to run a piece of software. Think of it as a complete box that includes the application code, runtime, system tools, libraries, and settings. This means you can run your application anywhere - on your laptop, a server, or in the cloud - and it will work exactly the same way.

### Why Use Containers?

![Docker Container](https://media.licdn.com/dms/image/v2/D4E12AQGWRz7lUifYVQ/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1699216771994?e=1735171200&v=beta&t=PdP6uOAQlZYupNkIKQuDFu2Wxir2DxubsqLvZ7kVYhQ)
*Source: [Mesut Oezdil on LinkedIn](https://www.linkedin.com/pulse/docker-containers-virtual-machines-explained-mesut-oezdil-shsfe/)*

Containers solve the common problem "But it works on my machine!" since they package everything the application needs. They're:

- Quick to start and stop
- Use fewer resources than virtual machines
- Easy to share with others
- Perfect for testing and development

### Creating Your First Container

Let's create a simple container that runs a "Hello World" web server. I'll show both CLI and Docker Desktop methods.

**Using the Command Line (CLI)**:

1. Pull an Official Image

   A Docker image is a lightweight, standalone package that includes everything needed to run a piece of software, including the code, runtime, libraries, and system tools. It's used to create containers, which are isolated environments for running applications.

   ```bash
   docker pull nginx
   ```

2. Create and Run a Container

   ```bash
   docker run --name my-first-container -p 80:80 -d nginx
   ```

   Let's break down this command:
   - `--name my-first-container`: Names your container
   - `-p 80:80`: Maps port 80 on your computer to port 80 in the container
   - `-d`: Runs the container in the background
   - `nginx`: The image to use

3. Check If it's Running

   ```bash
   docker ps
   ```

4. Visit `http://localhost` in your web browser to see the nginx welcome page.

**Using Docker Desktop**:

1. Open Docker Desktop
2. Click the **"Images"** tab on the left
3. Click the **"Run"** button next to the nginx image (or click **"Pull an image"** if you don't see it)
4. In the dialog that appears:
   - Enter a name (e.g., "my-first-container")
   - Set port mapping: Host port 80 → Container port 80
   - Click **"Run"**

That's it! Your container is now running and you can manage it through Docker Desktop's interface.

### Basic Container Management

**Using CLI**

Stop a Container:

```bash
docker stop my-first-container
```

Start it Again:

```bash
docker start my-first-container
```

Remove a Container:

```bash
docker rm my-first-container
```

**Using Docker Desktop**

In Docker Desktop:

1. Go to the **"Containers"** tab
2. Find your container in the list
3. Use the **play/stop** buttons to control it
4. Click the **delete** icon to remove it

#### Viewing Container Logs

**Using CLI**

```bash
docker logs my-first-container
```

**Using Docker Desktop**

1. Go to the **"Containers"** tab
2. Click on your container
3. Click the **"Logs"** tab to see the output
