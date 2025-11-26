# pipeline-docker

This is a continuation of my other repository that shows some best practices to create a Docker file:

https://github.com/tkeijock/dockerfile-best-practices


## 1- Overview
GitHub Actions is a continuous integration and continuous deployment (CI/CD) platform integrated directly into GitHub. It allows to automate workflows for building, testing, and deploying code. 

In this repository i use GitHub Actions to set up to automatically build a Docker image whenever code is pushed to a repository and then push that image to a container registry. This automation helps streamline development processes, reduces manual errors, and ensures that software is consistently built and deployed. This also allows me to build Docker images without having to install Docker on machines where I don’t have permission.

# 2 - Build and Push : Manual vs Docker Action

The ```docker/build-push-action@v6``` offers a streamlined and modern approach to building and deploying Docker images directly within GitHub Actions. Unlike using separate docker build and docker push commands, which require multiple steps and manual configuration, this action combines both operations into a single declarative block. 

It not only reduces complexity and boilerplate code but also introduces advanced capabilities that the manual approach lacks, such as automated layer caching, multi-platform builds, and dynamic tagging. These features significantly improve pipeline performance, speed up subsequent builds, and ensure consistent behavior across environments. 

By encapsulating best practices and eliminating repetitive commands, ```docker/build-push-action@v6``` provides a cleaner, more efficient, and production-ready solution compared to handling build and push operations separately.
