# Docker
- Docker is a open source platform
- Using for building, running, managing applications by packing them into a standardize unit called container. 
- Docker has two components docker container and docker images 

## Docker Container 
- A standardized, executable unit of software that packages up code and all its dependencies, so the application runs quickly and reliably from one computing environment to another.

## Docker Images 
- A read-only template that contains the application code, runtime, system tools, libraries, and dependencies needed to run an application. 
- It's a blueprint for creating Docker containers, which are isolated instances of an application and its environment.
- Tag is like a version or varient of docker images.

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
         docker pull hello-world
    ```
- docker images
    It shows all the available images 
    ```bash 
        docker images
    ```
- docker run IMAGE_NAME
    Used to buiild container from image
    ```bash
        docker run hello-world
    ```

- docker run -it IMAGE_NAME
    Used to buiild container from image which will run in interractive mode 
    ```bash
        docker pull ubuntu
        docker run -it ubuntu
    ```
    Now we are inside ubuntu container 
    ![docker image](image.png)

    Let's try some ubuntum commands 
    ```bash
        ls
    ```
    ![ls](image-1.png)

    Let's create a directory inside ubuntu container 
    ```bash 
        mkdir DIR1
    ```
    ![dir](image-2.png)

    Let's print different environment variables availbale inside ubuntu container
    ```bash 
        env
    ```
    ![env](image-3.png)

    Let's exit from the container
    ```bash 
        exit
    ```
    After the execution of exit command our container will stop running and back to system CLI


- docker ps 

     To check all the running container (ps - process status)
     ```bash 
     docker ps
    ```
    To check all the available container 
    ```bash 
    docker ps -a 
    ```

- dcoker start CONT_NAME or CONT_ID
- dcoker stop CONT_NAME or CONT_ID

    These two comands are used to start or stop the container 

    Starting the container with container Id 
    ```bash
        docker start e73ecd583380
    ```

    Stoppng the container with name
    ```bash 
        docker stop gallant_lovelace
    ```

- docker rmi IMAGE_NAME

    This command is used to remove or destroy image 

    NB: To delete the image we need to destroy the container first

- docker rm CONT_ID
    Used to remove container 

![alt text](image-4.png)


## Versions in docker Images

Docker versions are required when we want to pull a specific version of a image (image with specific tag).

Generally if we are pulling a image from docker hub without any tag then it will pull the latest version (image with latest tag) of that image.

```bash 
    docker pull mysql
```
It will pull the mysql with latest tag.


Here we are trying to pull mysql with specific tag or specific version
```bash
    docker pull mysql:8.0
```
![version](image-5.png)


- docker run -d IMAGE_NAME
 Here -d refers to dettach mode which simply refers to we can run a container in background 

 NB: By default all the container runs in attach mode 

This is compulsury to pass a root password by using -e while running the mysql image. For more details please follow https://hub.docker.com/_/mysql

```bash
    docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql
```
![sql](image-6.png)
![alt text](image-7.png)

 - docker run --name CONT_NAME -d IMAGE_NAME
 It is used to run an image with custom name.
``` bash
docker run --name mysql-older -d mysql:8.0
```

## Docker Image Layers
Docker images are built with a layered architecture, where each layer represents a filesystem change.
This layered approach offers significant advantages, including efficient storage, faster builds, and easier version control.
When we pulling two version from a single image then we have some commn layers. During installation it will show that layers alredy exists.

## Port Binding
Mapping host machine port with container port.

Before run this first remove the container if already exists

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-latest -p8080:3306 mysql
```
![alt text](image-8.png)

If once a host machine port binded with a container port then the host machine port will not bind with any container.
```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-older -p8080:3306 mysql:8.0
```
![alt text](image-9.png)

Container post may be same but local host machine port must be different.

## Troubleshoot Commands

These commands are use to check errors, which helps us to identify the root cause of the problem.

- docker logs CONT_ID or CONT_NAME
    To check logs of a specific container
```bash
    docker logs mysql-latest
    
    or 

    docker logs 29d086193144
```
![logs](image-10.png)


- docker exec -it CONT_ID or CONT_NAME bash
This command is used to access terminal inside a container (it will be in interractive mode)
```bash
    docker exec -it mysql-latest bash
```
![alt text](image-11.png)

We can exit from interractive cli using ```exit ``` command

- docker exec -it CONT_ID/bin/sh

In some container bash is replaced as sh, so to use the terminal inside a container we can use this command also.

## DOCKER and VIRTUAL MACHINE

                Application Layer ---- >>> OS kernel ---->>> Hardware

DOCKER -> Only virtualise application layer by using OS kernel, for this docker is faster, light weight

VM -> Virtualise OS kernel and Application layer

Docker desktop contains a small linux based virtual machine which helps us to run our applications.

## Docker Network
Docker has the ability to create isolated network which allows Docker containers to communicate with each other and with the outside world.

```bash 
docker network create NETWORK_NAME
```

To check available network in our system 
```bash
docker network ls
```

## Developing with Docker

- Creating a network 
``` bash 
docker network create mongo-network
```

- Setting up mongo and mongo-express

```bash
docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
```
![mongo](<Screenshot 2025-07-05 184643.png>)

Mongo is pulled sucessfully and connected to port number 27017, with username ```admin``` and password ```qwerty```

Now its time to pull mongo-express with the same network
```bash 
    docker run -d -p8081:8081 --name mongo-express --network mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017" mongo-express
```
![mongo-express](image-12.png)

- Now go to ```localhost:8081``` 

username -> admin

password -> pass

![admin](image-13.png)

Now we have a node js test application we run this 
```bash
node server.js
```

Goto localhost:5050 and fill the details and refresh the mongo-express dashboard we have a user list 
![users](image-14.png)

Now we are sucessfully connected with docker container.

## Docker Compose 
Docker compose is a tool for defining and running multicontainer application. 
This will be done with the help of a ```.yaml``` file. 

yaml - yet another markup language

**Advantages** : 
            - Standardize way of commands 
            - Easy to edit and modify

Indentation is most require in ```yaml``` file

![image](https://github.com/user-attachments/assets/262ba82f-2510-4f6d-8edc-be367d970e50)

```bash
    docker compose -f fileName.yaml ip -d
```
This up command is used when we want to create container in dettach mode.

```bash
    docker compose -f fileName.yaml down
```
This own command is used when we want to remove or delete container.

Now we run this ```yaml``` file by following these instructions.

1. It is necessary to check wheather the container already exists , if yes then delete it before running the ```yaml``` file.

2.Then run the compose command to create container 
```bash
    docker compose -f mongodb.yaml up -d
```
![image](https://github.com/user-attachments/assets/d7553173-4278-4778-87ba-812f481604a3)

Now we can see that two container are running on the spesified port
![image](https://github.com/user-attachments/assets/e2142081-1b85-4765-b483-cc8f8b086e98)


3. Now we can test is the container running or not by going to the ```localhost:8081``` port.
   Simillarly username will be ```admin``` and password will be ```pass```.
![image](https://github.com/user-attachments/assets/5fc70c8f-61b5-4696-b94a-721bd857bcf1)

We can see here there is no database with name ```datapirates```(we created before). It's because when we restart our container it became reset.
So we create the user in same way as before

4. Go to ```localhost:5050``` and get the doccument 
   
![image](https://github.com/user-attachments/assets/de9b5e9b-795b-4508-bf89-fd18dd904a1f)

5. If we want to remove the container with network.
```bash
    docker compose -f mongodb.yaml down
```
This will remove all the container with network.

## Dockerizing our App
Converting our ap to docker image then docker container.

Test app --->> docker image --->> container

A Dockerfile is a plain text file containing a set of instructions used to build a Docker image.

For mode you can follow https://spacelift.io/blog/dockerfile

### Important instruction of dockerfile

- FROM
Specifies the base image for the Docker image being built. It acts as the foundation upon which the rest of the image is built.
```bash
    FROM <base_image>
```

- WORKDIR
Sets the working directory inside the container for subsequent instructions like RUN, COPY, CMD, etc.
```bash    
    WORKDIR <path>
```

- COPY
Copies files and directories from the build context (your local machine) into the image.
```bash
    COPY <src> <dest>
```

- RUN
Executes shell commands during the image build process. These commands are run in new layers, and the results are committed to the image.
```bash
    RUN <command>
```

- CMD
Provides default instructions for running a container based on the image. It defines the command that will be executed when a container starts without any specific command-line arguments.
```bash
    CMD <command>
```

- EXPOSE
Informs Docker that the container listens on the specified network port at runtime. It's a form of documentation rather than a publishing mechanism.
```bash
    EXPOSE <port>
```

- ENV
Sets environment variables that can be used within the container.
```bash
    ENV <name> <value>
```

![layers ](image-15.png)

Now let's create a Dockerfile for our node application

![Dockerfile](image-17.png)

Now to build a docker image from this file we need to run this command 

```bash
    docker build -t testapp:1.0 .
```
![building](image-18.png)

Now we can check the available images by typing ```docker images``` in terminal


Now we can create a container from docker image

```bash 
    docker run testapp:1.0
```

![running image](image-19.png)

Also we can run our container in interractive mode 

```bash
    docker run -it testapp:1.0 bash
```
Now we can see that all the files are available inside our containeer.
![interactive mode](image-20.png)

Now we can see that we have node_modules folder inside our container which we dont want. If we want to skip this folder during image building then we can just write ```RUN npm install``` inside the ```Dockerfile```.

![node_modules](image-21.png)

Let's exit from the interractive terminal of container by typing ```exit``` inside it and enter. Now we have three docker images(we can als check by typing ```docker images``` inside the terminal). To run this three terminals simultaneously we can use the compose.

## Publishing Images in Dockerhub
1. Go to docker hub https://hub.docker.com/ and authenticate 

2. Go to My Profile --->> My hub -->> repository

3. Create a repo and copy the name (may be public or private)
![repo](image-22.png)

4. build with that repo 
```bash
    docker build -t rudranarayan01/testapp .
```
![building](image-23.png)

Now there is a new image in docker desktop wit name ```rudranarayan01/testapp```

5. Now docker login in terminal with docker hub credentials  (we are first logged in using doker desktop credentials so logout by entering ```docker logout```)

```bash
    docker login
```
![login](image-25.png)

Here  logged in using one time code(device verfication)

![sucess](image-24.png)

6. Now we can push the repo 
```bash 
    docker push rudranarayan01/testapp:latest
```

![doc hub pushed](image-26.png)

## Docker Volumes
 
Volumes are persistent data stores for containers. Containers stores data in virtual mode like if we restart the docker desktop all the data all the container will be reset. We can map multiple container in  a single volume.

- Persistent storage for containers: Data stored in a volume survives container restarts and deletions.
- Docker managed: Volumes are created, managed, and stored by Docker, typically in /var/lib/docker/volumes on Linux.
- Mounted to container paths: Data written to the mounted directory in the container goes to the volume.
- Data sharing: Multiple containers can access and modify the same volume.
- Easy backup and restore: Volumes simplify data backup and recovery processes.
- Named vs. Anonymous: Named volumes are user-defined and recommended, while anonymous volumes are automatically created and tied to the container's lifecycle.
- Volume drivers: Allow storing volumes on remote hosts or cloud providers. 

We use ```-v``` tag for defining volume.

```bash 
    docker run -it -v /Users/RUDRANARAYAN/Desktop/data:/test/data ubuntu 
```
Here u I'm giving the path where docker container will be store, and RUDRANARAYAN is my device name or hostname.

![volume](image-27.png)

Now go to inside data folder ``` cd data```, and then create two new file ``` touch index.html``` and ```touch server.js```.

To mount our project we can just add a volume inside ```mongodb.yaml``` file

![yaml update](image-28.png)

Then run the ```yaml``` file (must ensure that we have no running container)

```bash
    docker compose -f mongodb.yaml  up 
```

## Docker Volume Commands

```bash 
    docker volume ls
```
```bash
    docker volume create VOL_NAME
```

```bash 
    docker volume rm VOL_NAME
```
-------------------------------------------------------
Named Volumes(Managed by docker)
```bash 
    docker run -v VOL_NAME:CONT_DIR
```

Bind Mount(Managed by host only)
```bash 
    docker run -v HOST_DIR:CONT_DIR
```

Anonomous Volume (Managed by docker)
```bash 
    docker run -v MOUNT_PATH
```


Delete all ananomous volumes
```bash
    docker volume prune
```

