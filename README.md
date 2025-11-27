# Docker Build CI
In this repository, I use GitHub Actions to automatically build a Docker image from a Dockerfile and push it to Docker Hub. 

This setup solved a real issue I faced: I needed to build Docker images, but I didn’t always have access to machines where Docker could be installed.

By moving the build process to GitHub Actions, I can create images directly in the cloud and keep them available online, making distribution and deployment much simpler.

The default Docker Hub repository is:

https://hub.docker.com/repository/docker/tkeijock/apache_imp

Note: Docker Hub repository location can be altered by changing ```secret.docker_repo``` inside Github Actions.

This project reuses the Dockerfile from my other repository, applying those best practices here to build and publish the image:

https://github.com/tkeijock/dockerfile-best-practices


# 1- Overview

GitHub Actions is a continuous integration and continuous deployment (CI/CD) platform integrated directly into GitHub. It allows to automate workflows for building, testing, and deploying code. 

This repository uses a Docker build workflow located at:

```.github/workflows/docker-build.yml```

## 1.1 How to use "RUN Workflow"

To trigger the pipeline manually, open the Actions tab in this repository and select the workflow named "Build and Push Docker Image" from the list on the left.
On the workflow page, click the arrow beside "Run workflow" to expand the options, then press the **green Run workflow button** to start the process.

 https://github.com/tkeijock/docker-build-ci/actions

## 1.2 Expected Results

After the workflow finishes successfully, a new Docker image will be built and pushed to your Docker Hub account, that will have:

- A Image with latest tag — always updated with the most recent build.
- A Image with tag containing the commit SHA — a unique identifier for each build, allowing you to trace exactly which version of the code produced the image.
- A cache artifact — used by GitHub Actions to speed up future builds by reusing previously built layers instead of generating everything from scratch.

# 2 - Technical Architecture of the Pipeline

## 2.1 Build and Push : Manual vs Docker Action

```
name: Build and push image
  uses: docker/build-push-action@v6
  with:
    cache-from: type=registry,...
    cache-to: type=registry,...
```

The ```docker/build-push-action@v6``` offers a streamlined and modern approach to building and deploying Docker images directly within GitHub Actions. Unlike using separate docker build and docker push commands, which require multiple steps and manual configuration, this action combines both operations into a single declarative block. 

It not only reduces complexity and boilerplate code but also introduces advanced capabilities that the manual approach lacks, such as automated layer caching, multi-platform builds, and dynamic tagging. These features significantly improve pipeline performance, speed up subsequent builds, and ensure consistent behavior across environments. 

By encapsulating best practices and eliminating repetitive commands, ```docker/build-push-action@v6``` provides a cleaner, more efficient, and production-ready solution compared to handling build and push operations separately.

## 2.1 Build Caching
This workflow leverages the caching capabilities of docker/build-push-action@v6 by using the ```cache-from``` and ```cache-to``` options to store Docker layers directly in the container registry (or another cache backend).

>NOTE: Even though GitHub Actions uses ephemeral runners that are discarded after each execution, the cache persists outside the runner. This allows subsequent builds to reuse previously created layers, rebuilding only what has changed.

As a result, build times are dramatically reduced, network usage is minimized, and the CI pipeline becomes faster, more stable, and more resource-efficient.

To maximize cache efficiency, the Dockerfile is structured so that dependencies and other rarely changed components are defined in the early layers, while application code is copied last. This ensures that only relevant layers are invalidated on code changes. Additionally, a .dockerignore file excludes unnecessary files (such as documentation, test artifacts, and local configurations), preventing them from triggering unnecessary cache rebuilds.

## 2.2 Github Actions Secrets configuration

First, clone this repository locally using:

```git clone https://github.com/tkeijock/pipeline-docker ```

Then go to your repository on GitHub → Settings → Secrets and variables → Actions → New repository secret and add the required secrets: 

- DOCKER_USERNAME: Your Docker Hub username.

- DOCKER_PASSWORD: Your Docker Hub access token 

- DOCKER_REPO: The name of your Docker repository where the image will be pushed.

Tip: Using a Docker Hub personal access token instead of your actual password is more secure and is required for modern authentication with GitHub Actions.


