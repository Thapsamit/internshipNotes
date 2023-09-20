


# Docker

## Basic points about docker
- written in go programming languages
- Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

- Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allows you to run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, 

-Docker provides tooling and a platform to manage the lifecycle of your containers:
  - Develop your application and its supporting components using containers.
  - The container becomes the unit for distributing and testing your application.
  - When you’re ready, deploy your application into your production environment, as a container or an orchestrated service. This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.


## What can I use Docker for?
- Docker streamlines the development lifecycle by allowing developers to work in standardized environments using local containers which provide your applications and services. Containers are great for continuous integration and continuous delivery (CI/CD) workflows.

- Consider the following example scenario:
  - Your developers write code locally and share their work with their colleagues using Docker containers.
  - They use Docker to push their applications into a test environment and execute automated and manual tests.
  - When developers find bugs, they can fix them in the development environment and redeploy them to the test environment for testing and validation.
  - When testing is complete, getting the fix to the customer is as simple as pushing the updated image to the production environment.


## Docker architecture
![architecture](https://docs.docker.com/assets/images/architecture.svg)

### The Docker daemon:-
The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.


### The Docker client:-
The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.


### Docker registries:-
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.


## Images:-
An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.



## What is a container?
Simply put, a container is a sandboxed process on your machine that is isolated from all other processes on the host machine. That isolation leverages kernel namespaces and cgroups, features that have been in Linux for a long time. Docker has worked to make these capabilities approachable and easy to use. To summarize, a container:

Is a runnable instance of an image. You can create, start, stop, move, or delete a container using the DockerAPI or CLI.
Can be run on local machines, virtual machines or deployed to the cloud.
Is portable (can be run on any OS).
Is isolated from other containers and runs its own software, binaries, and configurations.



# To check docker running or not
```bash
docker info

```


## for running in ubuntu:-

```bash
sudo service docker start
```


```bash
sudo systemctl start docker
```

TO check status of docker
```bash
sudo systemctl status docker

```

# to build
```bash
 docker build -t <image-name> .  
```
# to run docker
```bash
docker run -p <HOST_PORT>:<CONTAINER_PORT> your-image-name
```

# To View running docker containers in aws ubuntu instance

```bash
docker ps
```

# To stop the running container in aws ubuntu instance
```bash
docker stop <container_id/name>

example

docker stop nodedocker
```


### To list all the docker images
```bash
docker images
```



## How to run commands like python manage.py shell if the project is running in docker

- Get container
```bash
docker ps
```
- Exec docker exec command
```bash
docker exec -it <container_id_or_name> bash


```

- Go to main app

```bash
cd /usr/src/app/

```

- run shell
```bash
python manage.py shell
```


### Installation in AWS EC2 ubuntu (Docker):-

- Below are steps to configure docker in AWS EC2 ubuntu








