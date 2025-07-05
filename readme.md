# Docker
- Docker is a open source platform
- Using for building, running, managing applications by packing them into a standardize unit called container. 
- Docker has two components docker container and docker images 

## Docker Container 
- A standardized, executable unit of software that packages up code and all its dependencies, so the application runs quickly and reliably from one computing environment to another.

## Docker Images 
- A read-only template that contains the application code, runtime, system tools, libraries, and dependencies needed to run an application. 
- It's a blueprint for creating Docker containers, which are isolated instances of an application and its environment.

## Installation 
- Install docker from official website and it's easy and just click and install 
- After installation check is it installed suuccessfully on our system by typing in cmd 
``` bash
docker
docker -v 
```

## Docker Hub
- Docker hub contains all the public collections of docker images https://hub.docker.com/

## Docker Commands 

- docker pull IMAGE_NAME
    It is used to pull image from docker hub 
    ```bash
         docker pull hello-world:nanoserver-ltsc2025
    ```
- docker images
    It shows all the available images 
    ```bash 
        docker images
    ```

- docker run -it IMAGE_NAME
- dcoker start CONT_NAME or CONT_ID
- dcoker stop CONT_NAME or CONT_ID