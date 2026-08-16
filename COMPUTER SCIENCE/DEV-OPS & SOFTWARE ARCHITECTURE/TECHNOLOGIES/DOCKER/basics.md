---
title: Docker
date: 2020-01-31
tags:
  - compose
  - docker
aliases:
---

What is Docker? What problems it solves, and why you should learn it right now!  
Docker is very useful for a lot of people, not only dev, even if you work with the IT even if you are an engineer, Docker is a must-know!  
Other than how to install docker on Windows, OSX, and Linux, we are going to see the basic of containers (what's in it, what is not in it running some example), then we are going to build some images, that are the building blocks of containers, then Networking for Docker, Storage stuff (Volumes).  
Then we get into Docker compose that is a tool that comes with docker but really it's designed for local dev and test!  
Once you understand all the basics of those tools we are going to talk about orchestration, which allows you to run containers in multiple servers that act sort/like a single server, and we start with "Swarm" because it comes with Docker, it is very easy to understand and it works really great in a lot of scenarios, and you'll learn about YAML files and how to configure multi-container solutions and distributed systems and microservices.  
But eventually, you'll want to jump into Kubernetes, which comes after Swarm, even if it is more complicated and does a lot more than Swarm.  

## Why Docker?

So one more time what is Docker? What is it about? Why it exists?  
Docker is a huge shift, one of the biggest in the latest years, and eventually, you are going to shift too.  
A little of history of major infrastructure shifts:

### Mainframe to PC (1990)

In the 90s we shifted from a traditional mainframe architecture that had been around for many years to a PC distributed architecture where we're pitting in Mac and Windows and Dos, and we changed our networks and putting in fiber Ethernet and TCP/IP etc..

### Virtualisation (2000)

Not even 10 years later (00s) we were changing again, in the data center and in our servers, we were shifting to virtualization because our servers were too powerful, and we had a lot of idle time, and we needed to get better utilization, so we started creating a lot of OS's inside a single machine, using virtualization with VMware, Hyper-V etc...

### Cloud (2010)

It started in 2007/8 when Amazon released the AWS solution. That was the first real idea we had around easy, cheap, disposable compute power that we could spin up and shut down really quickly from a website. No one had done that before, that easy, on that level of scale, and it really shifted the landscape.

### Host to Container (Serverless)

Then we have the next wave, and that's where we're at now which is containers. Now containers would also include server lists, or functions as a service (if you have heard of that), because all those things were made possible by containers and run inside containers to do their work.  
Fundamentally, it's the shift to containers as the next object of computing. You will see very soon, where it's the default for us to all assume that you're running your stuff in containers.

### Migration pain

All those changes in the course of the years mean migration, learn new tools, learn new terminology and a lot of more work, we all know that migrating is the hard part.

Unlike previous shifts, Docker is focused on the migration experience, the docker tools were really created from the "get go" with the developer in mind, which means that they work just as well for dev as they do for sysops and operators.

## What will Docker Do for Me?

Why you need docker? Why is the real benefit? It's all about speed!  
Like all the other major shifts, always about speed, about speed deployments, the speed of business and getting things done in a company for profit.  
You will see big benefits in all the areas when you adopt various tools of the ecosystem.

If you are not working with containers now you are probably working with multiple types of applications: frontend, backend, worker, middle tier, you have a lot of different things that all need to work together to run your software, and those all will have their own dependencies, their own requirements, they may even run on different operating system or different clouds.  
And then you have them running on developer machines, and in testing infrastructure, and then in production, and maybe it's in a data center and also in the cloud. It's complicated!

_We have a lot of different ways of dealing with that, but the way that containers do it, now, is consistent across the board, it allows you to package the same way regardless of your operating system, it allows you to distribute the software regardless of the setup, it allows you to run and test the software the same way everywhere you're running it: Mac, Windows, PC, cloud, data center, edge devices_... _it doesn't matter._

## Docker party

Docker started and still is, largely open-source, so every year on the month that Docker project was originally released (March) there is a party around the world, basically, all the various meetups around container technologies get together and celebrate docker releasing.

## Docker Other tools

Docker is the essence that you need to understand first but once you move beyond that you're probably going to want to check your other tools that you know, or what tools you might need to use to fill the gap in your toolset.  
There is a great series of posters and diagrams that describe a lot of these things at [https://landscape.cncf.io/](https://landscape.cncf.io/)

## Installing Docker

There are different flavors of Docker At this moment, and we need to go through real quick, to understand which one you need for the course.  
Let's learn about the free Community Edition Docker, Docker CE, and the EE version, Enterprise Edition. We won't actually cover much of that in this course, it's a paid version and it's usually for larger business, but we'll touch on it every so often but we won't be using it.

We are going also to know the difference between the Stable and Edge releases, and which one you should use.

### Which one to use?

Let's figure it out which one we need:  
Docker is not just a simple point and clicks installation sometimes because it's very big and contains a lot of features.

Your version matters: Since the evolution of containers is very fast and continuous, you need to make sure that you're on the most recent version.

There are three major types of installs: Direct, Mac/win, Cloud

### Direct installation

You just install it directly on a supported operating system, so that it runs on the kernel and that OS it's supported, and until 2016 was mostly just Linux, and then we started seeing innovations where the Raspberry PI could use it, and we saw it on mainframes, and then windows Server 2016 supported it natively.

### Mac and Windows 10

Installing for Mac/Win, is more of a not direct method, but more of a suite of tools including a GUI and seeings, preferences, to make it really easy for you to develop. Since is not supported natively, Docker has to start a Virtual Machine to run the containers in.

### Cloud Installing

The cloud version is really what I would say are AWS, Azure, or Google versions, so Docker for AWS or Docker for Azure etc... And those versions come with features specific to that cloud vendor, for example for AWS side, it'll come with the cloud formation template, and with persistent storage options for storing your databases.

Of course, we are going to start with the Win/Mac/Linux version but in the end we will start to create Swarms in the cloud, and we will see the options at that point on how to do that.

### Edge vs Stable

Edge means Beta in the Docker world, it comes out every month, and it is only supported for a month until the next beta come out. You may not want to do that, when you're downloading Docker by default, you will get the Stable version that comes out once a quarter, and is supported for the next 4 month, and then is not supported and you need to update it (or pay the EE version)

### Installing on Windows (finally)

There are two types of containers now on Windows, which is really cool, we have the option for Linux containers, or windows containers. This course will talk about Linux containers because the windows one it's new. But most of the concepts are the same, you are going to use them the same way, you are going to manage them the same way, it just matters that binaries that are running inside them, you know, SQL, EXE vs MySQL on Linux or something. So when I say container I mean Linux container, and I will specifically call for Windows containers.

The best version of Docker on Windows is, of course, Docker for Windows, that is actually an addition of Docker and it got a whole install that we'll see a moment and it's really great on Windows 10 (Pro, Enterprise Only)

If you don't have Hyper-v enabled on your machine, when installing Docker it will be enabled and the system need a restart.

On some laptop and very rarely on PC you might need to enable virtualization on the BIOS of your machine, so if you have an error lookup on the internet and you probably find out that you need to go to the BIOS.

once you have installed Docker, and that it is running you can open PowerShell and do:

```
docker version 
```

And you should see it running. And if you open Hyper-v you will see the virtual machine running (on a MobyLinuxVM)

### Share the harddrive

If Docker didn't ask you to share a hard drive, or if your code is not in C:\\ you need to go to setting->Shared drives and share the drive where you have your code.

### Adjust the resources

Still, in setting on the Advanced tab, you can choose to adjust the resources dedicated to Docker, so you can choose how many cores you want to give to him and how much memory (RAM)

## CLIENT - SERVER - ENGINE

When installing docker you can see the version of docker using

```
$ docker version
 Client:
  Version:           19.03.8-ce
  API version:       1.40
  Go version:        go1.14.1
  Git commit:        afacb8b7f0
  Built:             Thu Apr  2 00:04:36 2020
  OS/Arch:           linux/amd64
  Experimental:      false
 Server:
  Engine:
   Version:          19.03.8-ce
   API version:      1.40 (minimum version 1.12)
   Go version:       go1.14.1
   Git commit:       afacb8b7f0
   Built:            Thu Apr  2 00:04:16 2020
   OS/Arch:          linux/amd64
   Experimental:     false
  containerd:
   Version:          v1.3.4.m
   GitCommit:        d76c121f76a5fc8a462dc64594aea72fe18e1178.m
  runc:
   Version:          1.0.0-rc10
   GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
  docker-init:
   Version:          0.18.0
   GitCommit:        fec3683
```

As you can see this return us two parameters:  
Client and Server!  
The client is the CLI, the software that "talks" with the Server, while the Server (also called the engine) is the main component that makes docker run!  
It's a service (or a daemon in Linux).  
Ideally you want them both at the same version, and of course at the latest version.

By the way, if the server part is not showing, it's probably because you didn't access it through superuser privileges, so you need to give authorizations to your user, or just use "sudo" when you need to work with docker.

### Docker info

If you do:

```
$ docker info
 Client:
  Debug Mode: false
 Server:
  Containers: 0
   Running: 0
   Paused: 0
   Stopped: 0
  Images: 0
  Server Version: 19.03.8-ce
  Storage Driver: overlay2
   Backing Filesystem: 
   Supports d_type: true
   Native Overlay Diff: false
  Logging Driver: json-file
  Cgroup Driver: cgroupfs
  Plugins:
   Volume: local
   Network: bridge host ipvlan macvlan null overlay
   Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
  Swarm: inactive
  Runtimes: runc
  Default Runtime: runc
  Init Binary: docker-init
  containerd version: d76c121f76a5fc8a462dc64594aea72fe18e1178.m
  runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
  init version: fec3683
  Security Options:
   apparmor
   seccomp
    Profile: default
  Kernel Version: 5.3.18-1-MANJARO
  Operating System: Manjaro Linux
  OSType: linux
  Architecture: x86_64
  CPUs: 4
  Total Memory: 3.753GiB
  Name: dell-sp
  ID: ETOK:2XDZ:X52V:NHTR:DEWT:AZJ4:32CR:Z5NP:JVKU:QDPQ:GBN3:JXXB
  Docker Root Dir: /var/lib/docker
  Debug Mode: false
  Registry: https://index.docker.io/v1/
  Labels:
  Experimental: false
  Insecure Registries:
   127.0.0.0/8
  Live Restore Enabled: false
```

You will get a lot more information about your docker engine.  
Infos about configuration and setup, our engine, the number of container running and

### WHAT COMMAND?

If you want to see all (not really all but most) of the command you can use on docker, just write docker on your CLI and hit enter:

```
Usage:    docker [OPTIONS] COMMAND
 A self-sufficient runtime for containers
 Options:
       --config string      Location of client config files (default
                            "/home/simonep/.docker")
   -c, --context string     Name of the context to use to connect to the
                            daemon (overrides DOCKER_HOST env var and
                            default context set with "docker context use")
   -D, --debug              Enable debug mode
   -H, --host list          Daemon socket(s) to connect to
   -l, --log-level string   Set the logging level
                            ("debug"|"info"|"warn"|"error"|"fatal")
                            (default "info")
       --tls                Use TLS; implied by --tlsverify
       --tlscacert string   Trust certs signed only by this CA (default
                            "/home/simonep/.docker/ca.pem")
       --tlscert string     Path to TLS certificate file (default
                            "/home/simonep/.docker/cert.pem")
       --tlskey string      Path to TLS key file (default
                            "/home/simonep/.docker/key.pem")
       --tlsverify          Use TLS and verify the remote
   -v, --version            Print version information and quit
 Management Commands:
   builder     Manage builds
   config      Manage Docker configs
   container   Manage containers
   context     Manage contexts
   engine      Manage the docker engine
   image       Manage images
   network     Manage networks
   node        Manage Swarm nodes
   plugin      Manage plugins
   secret      Manage Docker secrets
   service     Manage services
   stack       Manage Docker stacks
   swarm       Manage Swarm
   system      Manage Docker
   trust       Manage trust on Docker images
   volume      Manage volumes
 Commands:
   attach      Attach local standard input, output, and error streams to a running container
   build       Build an image from a Dockerfile
   commit      Create a new image from a container's changes
   cp          Copy files/folders between a container and the local filesystem
   create      Create a new container
   diff        Inspect changes to files or directories on a container's filesystem
   events      Get real time events from the server
   exec        Run a command in a running container
   export      Export a container's filesystem as a tar archive
   history     Show the history of an image
   images      List images
   import      Import the contents from a tarball to create a filesystem image
   info        Display system-wide information
   inspect     Return low-level information on Docker objects
   kill        Kill one or more running containers
   load        Load an image from a tar archive or STDIN
   login       Log in to a Docker registry
   logout      Log out from a Docker registry
   logs        Fetch the logs of a container
   pause       Pause all processes within one or more containers
   port        List port mappings or a specific mapping for the container
   ps          List containers
   pull        Pull an image or a repository from a registry
   push        Push an image or a repository to a registry
   rename      Rename a container
   restart     Restart one or more containers
   rm          Remove one or more containers
   rmi         Remove one or more images
   run         Run a command in a new container
   save        Save one or more images to a tar archive (streamed to STDOUT by default)
   search      Search the Docker Hub for images
   start       Start one or more stopped containers
   stats       Display a live stream of container(s) resource usage statistics
   stop        Stop one or more running containers
   tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
   top         Display the running processes of a container
   unpause     Unpause all processes within one or more containers
   update      Update configuration of one or more containers
   version     Show the Docker version information
   wait        Block until one or more containers stop, then print their exit codes
 Run 'docker COMMAND --help' for more information on a command.
```

## CONTAINERS

Containers are the fundamental building block of the Docker toolkit and that's one of the first things we are going to learn, we are not going to build images or do any advanced stuff until once we've learned what containers are.

### IMAGE VS CONTAINER

An image is the library, the binaries, the source code that makes up your application! This could be PHP, MySQL, PostgreSQL, Node, pretty much anything ...

The container is a running instance of that image, so the actual running thing based on an image, you can have different running containers of the same image.

### REGISTRIES

Ok, but where we get the images that we need? We use registries, like npm for node, or repositories for Linux.

the one for docker is [hub.docker.com](https://hub.docker.com/)

### BUILD THE FIRST CONTAINER

Let's build our first container:

```
$ docker container run --publish 80:80 nginx
```

(don't worry we are going to explain everything later)  
This is the result:

```
Unable to find image 'nginx:latest' locally
 latest: Pulling from library/nginx
 afb6ec6fdc1c: Pull complete 
 b90c53a0b692: Pull complete 
 11fa52a0fdc0: Pull complete 
 Digest: sha256:30dfa439718a17baafefadf16c5e7c9d0a1cde97b4fd84f63b69e13513be7097
 Status: Downloaded newer image for nginx:latest
```

Now if you go to your web browser and type `localhost` you will be welcomed by nginx! So in no time, we have built an instance of Nginx (that could have been Apache too) and it is up and running.  

So what just happend?  
In when we pass the command, Docker is looking for an **image** (remember?) called Nginx and if he doesn't find it on the local machine, it will look in the repository (hub.docker.com) and pull the latest version of that image, and it starts it! As a new process(run), in a new container!

The `--public` part of the code expose my local port 80 on my local machine, and sends all traffic from it to the executable (docker instance) inside that container on port 80..  
So `80:80` means actually From 80(local) to 80(container).  
Remember that containers are like micro computer with their Os and their network.

### DETACH or -d

So, if we close the terminal (at least on Linux) the container will stop working, (also if you press `ctrl+c`.  
If you want your container to run like a service, so that is not liked to a terminal you can use the `--detach` option while creating the command:

```
$ docker container run --publish 80:80 --detach nginx
```

And as response we have the container ID:

```
a66dfb13ee401d5b6bdb3f7226162b4653fc5aecd97dac88702c5837af601a18
```

Now also know that every time you create a new container, it will have a new ID!

### LISTING CONTAINERS

So we know how to create differents containers, but now we want to know how to see what are the container created, to do so you can just do:

```
docker container lsCONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMESa66dfb13ee40 nginx "nginx -g 'daemon of…" About a minute ago Up About a minute 0.0.0.0:80->80/tcp agitated_joliot
```

The command `docker container` will return us some informations about the containers that are up and running.

### STOPPING A CONTAINER

Now we need to know how to stop a container.  
In order to stop a container you can just send:

```
docker container stop a66
```

What is a66? Is the beginning of the ID of the Container that we want to stop! You don't need to write all the ID down, just what is needed to understand that is the correct and unique ID.

### LIST ALL CONTAINERS AND NAMES

Now let's try this:

```
docker container ls -aCONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMESa66dfb13ee40 nginx "nginx -g 'daemon of…" 9 minutes ago Exited (0) 2 minutes ago agitated_joliot65edd8c7c563 nginx "nginx -g 'daemon of…" About an hour ago Exited (0) 10 minutes ago thirsty_shockley
```

using the option `ls -a` we list all the existing containers not just the one that is running. Im not sure if you can see well the result, but there is a name at the end of each line (thirsty\_shockley and agitated\_joliot) those are like ID, but are names.

Like ID because they need to be unique, and if we don't specify a name for container, docker will give one for us, and he does that using an open source database of names, surnames, adjectives, or verbs, of notable hackers or scientists(just a funny story)

Let's try to give a container a name:

```
docker container run --publish 80:80 --detach --name webhost nginx
```

We use the opion `--name` followed by the name we want to give and we can check the result with `docker container ls -a`

You can use the name to stop the container:

```
docker container stop webhost
```

And the container will stop. And if you want that container to run this time you use `start`

```
docker container start webhost
```

This way you don't have to re-parameter the container but just call his name.

### REMOVING CONTAINERS

All containers are actually "stored" in your computer, even if they are not running, if you want to delete a container you can simply do:

```
docker container rm container_name 
```

### SEE CONTAINER LOG

Another useful use of the name, is when you want to see the log of a container.  
If you detach your container, in order to see the log of your container you must send via CLI:

```
docker container logs containername
```

This will show us the log of that container.

## WHAT HAPPENS IN DOCKER CONTAINER RUN?

Let's focus now on what happens when you a docker container run.

First there is a misconception that Docker is just a running container and that is his main job, but there is actually a lot more that is happening in the background that does in addition to those containers executing commands.

We already know that when we do a run, docker will look for that image and if it doesn't find it locally in the image cache, it looks on his repositories, then it downloads it and store it in the local image cache.

Then when the image is ready to go, it's going to start up a new container BASED on that image.  
It's not going to make a copy of that image, it's actually going to just start a new layer of changes, right on top of where that image left off, and it's going to customize the networking, so it is going to give a specific virtual IP address that's inside a Docker Virtual Network, and It's actually open up the port that we specified.

If we didn't specify that Publish command, it is not going to open any port at all, and since we did the `80:80` we are specifying to take the port 80 on the host and forward all the traffic to the port 80 of the container.

Then that container finally start using a command specified in the Dockerfile (we will see this later).

Those are also all the thing that we can change with the command that we run!

## CONTAINER VS VIRTUAL MACHINE

When you look online, to understand better what a container is, people usually compare them to virtual machines, and yes there are some similarities, but there are so many things that are just not the same.

Containers are more just like a process that VMs, in fact if you do:

```
docker top container_name
```

You will see that that container has a PID (process ID) and you can find this process id also if you see and list all the process id in your machine (if you use Linux, if you use windows or mac docker create a virtual machine that run docker, but the process will be on that virtual machine) .  
So as you can see, the PID is not hided in a virtual machine, because it is a process of the host machine.

## INSIDE A CONTAINER!

### USEFUL COMMANDS

Here's a list of usefull comands to see what is going inside a container:

```
docker container top container_name_or_id  -process list in a container
docker container inspect container_name_or_id  -details of container config
docker container stats container_name_or_id  -performance stats for all container

```

### SHELL OF THE CONTAINER

One of the most common things to do with a container is to pass command via the shell, do to so you may think that you need to run ssh to connect to the process, like you would do in a virtual machine env, but there are some tool that we can use directly with docker:

#### DOCKER CONTAINER RUN

The `docker container run` command let's us actually do the trick:

```
docker container run -it 
```

The `-it` option is actually 2 separate otions, you can see that with `--help`!  
The `-t` give us a pseudo-TTY (or a prompt) simulating a real terminal.  
The `-i` keep STDIN open (the STDIN is the session).

Let's see this at work:

```
docker container run -it --name proxy nginx bash
root@a490b73d82e9:/#
```

As you can see, we create a new docker container and we name it proxy, then we give the command bash, to indicate that we want to use the bash terminal

At the end of the run command, you can see that you can send commands and arguments, (if you use `--help`), an image has a default command in it that we can configure (later), but if you want to change what is run on startup, you can do right from the run line (bash).  
And as you can see the user that we use inside the container is root!  
And if you try to do a `ls` you can see that you have a bunch of folders you can access

From here so you can change config files, download packages from the internet, basically anything that you can do during your common administrative thing from a normal shell.

To get out of that shell, just type exit like you would do from a normal shell, but doing so, you also exited the container! Why? because we changed the command that run when we run the container! So automatically exiting the script will stop the container.

#### INSTALLING UBUNTU

You could if you need download install and run an Ubuntu image!  
Of course the Ubuntu image from the hub.docker.com is way more lightweight than a normal distribution, but you could in theory, install al the missing component that you may need.

### RESTART A CONTAINER WITH A CLI

We can use the `docker container start`!  
With the difference that you don't use `-it` but `-ai`

```
docker container start -ai proxy
```

and you can access the shell inside that container, bu what if you just want to access the shell of a already running container?

We can do that too using the `exec` command: This command create another process inside the container, so when you exit this process the container will not stop. Let's try:

```
docker container exec -it mysql bash
```

And if we exit the process, this will not stop
