---
title: "Docker-compose"
date: "2020-07-08"
tags: 
  - "docker"
  - "docker-compose"
  - "linux"
---

Those are the notes coming from this video : [https://youtu.be/DX1T-PKHKhg](https://youtu.be/DX1T-PKHKhg)

YAML Tutorial : [http://www.simonepanebianco.fr/yaml/](http://www.simonepanebianco.fr/yaml/)

File YAML Version Matrix : [https://docs.docker.com/compose/compose-file/compose-versioning/](https://docs.docker.com/compose/compose-file/compose-versioning/)

As you know Docker is a tool which is used by developer and operation teams to create and automate the deployment of applications in lightweight containers so that applications can work efficiently in different environments

We have to remember that a container is just software and it has dependencies that are within that application and are managed by Docker.

![](images/image-1-1024x663.png)

So, first, we have a Docker File that creates a Docker image. The Docker File is a text-based script that has all the command that allows you to run the appropriate technologies within the docker image.

The docker image allows you to be able to have a wide group of people and consistently run the same environments and if you want to be able to share that Docker image, you would do so by uploading to Docker hub. The Docker hub can be available either publicly through the dock in the public docker hub website, or you can set up your own prior Enterprise Docker Hub. And then from docker Hub people can pull their own docker image and build new containers that are consistent to the original docker container that you have created.

A great way of being able to have your entire team scale up easily and effectively.

![](images/image-2-1024x380.png)

**But one of the common questions you can have is how you can use two or more containers in a single service?** (Ex: Apache+DB)

![](images/image-3-1024x1014.png)

Achieve this task using only Docker is a difficult task, you can do it but it is just very time-consuming and not a very good use of your productivity.  
But with Docker Compose you can actually do all process quite quickly and easily, and that's the reason for the existence of docker-compose.

Docker compose is very good for developing (just development not production) for multi micro-service style software.

The way docker compose runs is that it works as a single service, so you have the docker compose running multiple containers but the perception is of a single service run.  
The thing that is great is that each of these containers run in isolation, but they can also interact with each other (unlike a normal docker environment).

### Docker Compose Files

The Docker compose files are easy to write, they are written with a scripting language called YAML (Yet Another Markup Language) an XML based language, very easy to use and it is used by a lot of open source tools in the devOps environment.

http://www.simonepanebianco.fr/yaml/

Using the Docker Compose File you can run all your container with just a line of code.

![](images/image-1024x300.png)

So the benefits of docker compose are that:

**Single Host Deployment** : you can deploy all your containers inside a single machine.

**Quick And Easy configuration** : Using the YAML Scripts

**High Productivity** : Don't waste time trying to configure docker contaienrs one by one.

**Security** : Each of those containers are isolated from each other, they are controlled with the same level of security of a traditional docker image.

![](images/image-4-1024x331.png)

### Differences avec Docker Swarm

Docker Swarn and Docker compose may seems like they are the same thing, but they are really different.

The main difference start when you want to scale your solution. While Docker compose let you create multiples container only **on a single** host, Docker Swarm let you create containers also on **multiple hosts(clustering).**  
Divide the operations in differents hosts make sense in an operation environment, but not very much in a developer environment.

Docker Compose also uses YAML to manage the containers while Docker Swarm does not.

![](images/image-5-1024x450.png)

### Basic commands of Docker Compose

![](images/image-6-1024x584.png)

```
//Start all services with a command:
docker-compose up

//Stop all services with a command:
docker-compose down

//Command to run Docker Compose file
docker-compose up -d

//Command to list down all the process
docker ps

//Command to scale a service
Docker Compose up -d --scale

//Command to use YAML file to configure application services
Docker Compose.yml

//Command to install Docker Compose using pip:
pip install -U Docker-compose

//command to check the version of Docker Compose
docker-compose -v
```

### How tot use Docker Compose

Of course this will work if you have docker and docker compose installed in your machine.

Create folder for the testing (I have called it 'DockerComposeTes't), and create a docker -compose.yml inside this folder and we are going to edit it.

What we want to do is create 2 containers, one for the nginx web server , and the other one is for the database.

If you want you can go to docker hub ([https://hub.docker.com/](https://hub.docker.com/)) and see for those images, because the docker-compose will look for those images and download them.

```
service:
web:
image:nginx
database:
image:redis
```

Now, let's try to run:

```
docker-compose config

//Result
ERROR: yaml.scanner.ScannerError: while scanning a simple key
in ".\docker-compose.yml", line 3, column 1
could not find expected ':'
in ".\docker-compose.yml", line 4, column 1
```

`docker-compose config` allows us to validate and view the Compose file, and in this case is returning us an error, because our file .yml is not properly formatted. This is how it should be formatted:

```
service:
  web: image:nginx
  database: image:redis
```

So let's try this one out:

```
docker-compose config

//RESULT
ERROR: The Compose file '.\docker-compose.yml' is invalid because:
Unsupported config option for service: 'web'
```

Another error! This time the problem is different, there is still a config error in the .yml file, but is not a parsing one.

One of the thing that you have to do when you create a .yml file for docker-compose is to have the appropriate version number of the file, and that depends on the docker version that you have installed in your system.

So we have to find which is the proper version that we need to use:

```
docker -v
Docker version 19.03.5, build 633a0ea
```

In my case i'm using a version of docker 19.03.5, now I can check with the documentation of docker([here](https://docs.docker.com/compose/compose-file/compose-versioning/)) which version of the file this should be. In my case 3.8

Now we add this information inside our .yml file:

```
version: "3.5"
service:
  web:
    image: nginx
  database:
    image: redis
```

Let's see now:

```
docker-compose config

//RESULT
ERROR: Invalid top-level property "service". Valid top-level sections for this Compose file are: version, services, networks, volumes, secrets, configs, and extensions starting with "x-".
```

We need to add an s to `service:`! As you can see you have a limited top-level property that you can use: version, services, networks, volumes, secrets, configs, and extension starting with "x-", let fix this one and try one more time:

```
version: "3.5"
services:
  web:
    image: nginx
  database:
    image: redis
```

```
docker-compose config

services:
database:
image: redis
web:
image: nginx
version: '3.5'
```

It returns us the config file, so the config file is correct!  
The only difference is that the version of the docker file is now at the bottom, and that is just how docker compose works, it pulls out the services and the image, and put the version number at the bottom.

And we can finally test if everything works doing:

```
docker-compose up -d
```

This will download the image from the docker hub, then build the container, and we are ready to go.

And if we want to close all those container we just do:

```
docker-compose down
```

### What We can write inside a .yml file

Well now that we know how to create a docker-compose .yml file, the consequent question is how to configure our containers.  
In the documentation you can have more informations about this file ([https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/))

#### **image**

Using `image:` we specify what image we are looking for in docker hub.

#### build

Configuration options that are applied at build time.

`build` can be specified either as a string containing a path to the build context:

```
version: "3.8"
services:
  webapp:
    build: ./dir
```

Or, as an object with the path specified under [context](https://docs.docker.com/compose/compose-file/#context) and optionally [Dockerfile](https://docs.docker.com/compose/compose-file/#dockerfile) and [args](https://docs.docker.com/compose/compose-file/#args):

```
version: "3.8"
services:
  webapp:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
```

If you specify `image` as well as `build`, then Compose names the built image with the `webapp` and optional `tag` specified in `image`:

```
build: ./dir
image: webapp:tag
```

This results in an image named `webapp` and tagged `tag`, built from `./dir`.

> Note when using docker stack deploy
> 
> The `build` option is ignored when [deploying a stack in swarm mode](https://docs.docker.com/engine/reference/commandline/stack_deploy/) The `docker stack` command does not build images before deploying.

#### networks:

Usually, when you create a container, this is automatically attached to a network called "bridge", and the container can communicate to each other, if they are in the same network, using as **host** the **container's name**.

But you can also specify the network for the container to attach, and give the same network to one or more containers. Let's see an example:

```
//.yml file
version: "3.5"
services:
  web:
    volumes:
      - ./:/usr/src/app
    ports:
      - "54321:54321"
    build: ./docker/node
    networks:
      - proxynet
networks:
  proxynet:
    name: custom_network
    external: true
```

Here you can see that we have a service called web, and we can we assign a network called "proxynet", using the `networks` key, but this name ("proxynet") is just a reminder of what we define in the first level `networks`: in fact, we give the name "custom\_network" to the network, but we refer to this "custom\_network" by the name "proxynet".

So if you want another container to be on the same network you can add to his networks `- proxynet`

I also added another key to our `proxynet` network, the key `external: true`: that's because I actually start this network in another docker-compose, and this is an "external" network! Why is this necessary? Because if I don't say that the network is external, when I stop the docker-compose, docker will try to delete the network even if there are other containers in the network. This will produce an error, not a fatal one, but still an error.
