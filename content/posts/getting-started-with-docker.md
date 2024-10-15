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

From installation to launching your first container, we'll walk you through the fundamentals of Docker in this tutorial, assisting you in taking the initial steps towards containerization and contemporary application development. This guide might serve as a good starting point for anyone interested in learning how Docker can streamline their workflow, be they a developer, system administrator, or just inquisitive.

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
