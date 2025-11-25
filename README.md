# pipeline-docker

This is a continuation of my other repository that shows some best practices to create a Docker file:

https://github.com/tkeijock/dockerfile-best-practices


## 1- Overview
GitHub Actions is a continuous integration and continuous deployment (CI/CD) platform integrated directly into GitHub. It allows to automate workflows for building, testing, and deploying code. 

In this repository i use GitHub Actions to set up to automatically build a Docker image whenever code is pushed to a repository and then push that image to a container registry. This automation helps streamline development processes, reduces manual errors, and ensures that software is consistently built and deployed. This also allows me to build Docker images without having to install Docker on machines where I don’t have permission.
