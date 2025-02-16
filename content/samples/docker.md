---
title: "Migration to Kubernetes"
date: 2025-02-01
---

## Contents

* [Introduction](#introduction)
* [Docker](#docker)
* [Prerequisites](#prerequisites)
* [Build](#build)
* [Run](#run)
* [Deploying to Kubernetes](#kubernetes)

## Introduction  <a name="introduction"></a>

We are moving our services into cloud infrastructure and starting to use Kubernetes.

[Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is a container-orchestration system for automating application deployment, scaling, and management.

[Docker](https://www.docker.com/resources/what-container) is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.

This instruction will be helpful for developers who have never worked either with Kubernetes or with Docker.

This instruction describes how to:
- prepare image with an application;
- build and publish image;
- run your application inside container.

In the same repository with this article, all mentioned files are located.

See also:
- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)

## Docker basics <a name="docker"></a>

Docker is a client-server application with the ability to package and run an application in an isolated environment called a container.

**Image**

An image is a read-only template with instructions for creating a container. Often, an image is based on another image, with some additional customization.
To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image.

**Container**

А сontainer is a runnable instance of an image. A container is relatively well isolated from other containers and its host machine.
You can run several instances of one image on the same host.

**Registry**

Storage of images. Can be private or public. The largest public registry is `hub.docker.com`. It will be used by default.

See also:
- [Docker overview](https://docs.docker.com/engine/docker-overview/)

## Prerequisites <a name="prerequisites"></a>

Docker is available for different platforms but this user guide is primarily for the Windows platform.
General system requirements can be found in the [Docker Installation Guide](https://docs.docker.com/docker-for-windows/install/#system-requirements) article.
In addition to this, you should install:
- [Docker](https://docs.docker.com/docker-for-windows/install/).
- [Git](https://git-scm.com/download/win) (optional, since you can download this repository as .zip).

## Build <a name="build"></a>

The following step-by-step guide describes how to wrap your application inside an image.
For example, we will take a Python application called `ExampleApp`, which accepts HTTP-requests on port 8000.

1. Create folder structure as follows:

   ```text
   quickstart_docker/  # root directory of project
   ├──application/     # application source code located here
   └──docker/          # Docker-related files
      └──application/  # contains Dockerfile
   ```

   To create such structure you can use command:

   ```bash
   mkdir quickstart_docker \
   mkdir quickstart_docker/application \
   mkdir quickstart_docker/docker \
   mkdir quickstart_docker/docker/application
   ```

2. Put all your source code into `quickstart_docker/application`.

   In this example, create `application.py` file with the following content:

   ```python
   import http.server
   import socketserver

   PORT = 8000

   Handler = http.server.SimpleHTTPRequestHandler

   httpd = socketserver.TCPServer(("", PORT), Handler)

   print("serving at port", PORT)
   httpd.serve_forever()
   ```

3. Configure container for your application.
Image content defined in a configuration file called [Dockerfile](https://docs.docker.com/engine/reference/builder/).
In our scenario, use the following file:

   ```text
   FROM python:3.5
   WORKDIR /app
   COPY ./application /app
   EXPOSE 8000
   CMD ["python", "/app/application.py"]
   ```

   - `FROM python:3.5` — inherit from the public image from Docker Hub registry with python and all dependencies installed.
   - `WORKDIR /app` — create a working directory with the application.
   - `COPY ./application /app` — copy local `application` directory to the container's `app`.
   - `EXPOSE 8000` — make port 8000 of container available outside this container.
   - `CMD ["python", "/app/application.py"]` — execute `python /app/application.py` during every container launch.

4. Build an image using created Dockerfile.

   For this execute [`docker build`](https://docs.docker.com/engine/reference/commandline/build/) command:

   ```bash
   $ docker build . -f docker/application/Dockerfile -t exampleapp
   ```

   Arguments:
   - `.` — working directory;
   - `-f docker/application/Dockerfile` — path to your Dockerfile;
   - `-t exampleapp` — the name of the created image.

5. Check that the new image has appeared.

   To list all images existing in your local registry, use the [`docker images`](https://docs.docker.com/engine/reference/commandline/images/) command:

   ```bash
   $ docker images
   ```
   ```text
   REPOSITORY             TAG           IMAGE ID          CREATED                   SIZE

   exampleapp              latest          83wse0edc28a         2 seconds ago       153MB
   python                 3.6             05sob8636w3f        6 weeks ago           153MB
   ```

6. Push your image to the remote registry.

   Firstly, you should authorize in remote registry via [`docker login`](https://docs.docker.com/engine/reference/commandline/login/) command:

   ```bash
   $ docker login
   ```

   To push some image from your local registry to a remote one, use the [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) command:

   ```bash
   $ docker push exampleapp
   ```

## Run <a name="run"></a>

Now you or anyone else can run your application with a single command.

To run (and pull if needed) image, you can use the [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) command:

```bash
$ docker run -p 8000:8000 exampleapp
```

Arguments:
- `-p 8000:8000` — port mapping;
- `exampleapp` — the name of the image.

Now your application is available on http://localhost:8000/.

## Deploying to Kubernetes <a name="kubernetes"></a>

Learn how to deploy an image to Kubernetes:
- [Deploying to Kubernetes](https://docs.docker.com/get-started/part3/)
