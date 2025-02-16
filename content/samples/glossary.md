---
title: "Glossary"
date: 2025-02-01
---

## Docker Basics

Docker is a client-server application with the ability to package and run an application in an isolated environment called a container.

### Image

An image is a read-only template with instructions for creating a container. Often, an image is based on another image, with some additional customization. To build your image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image.

### Container

A container is a runnable instance of an image. A container is relatively well isolated from other containers and its host machine. You can run several instances of one image on the same host.

### Registry

A registry is where images are stored. It can be private or public. The largest public registry is [hub.docker.com](https://hub.docker.com), which is used by default.
