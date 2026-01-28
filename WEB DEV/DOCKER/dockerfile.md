---
title: "DOCKERFILE"
date: "2020-07-08"
tags: 
  - "docker"
  - "docker-compose"
---

More infos about dockerfile: [https://github.com/wsargent/docker-cheat-sheet#dockerfile](https://github.com/wsargent/docker-cheat-sheet#dockerfile)

[https://docs.docker.com/engine/reference/builder/#environment-replacement](https://docs.docker.com/engine/reference/builder/#environment-replacement)

A dockerfile is a file that contains instructions to build our container, you write the dockerfile, then run `docker build` and docker will create the container based on the instruction written in the dockerfile.  
So it's useful if you want to create your own image.

**Building the dockerfile**

The first step is, of course, create a dockerfile! The file doesn't need to be called "dockerfile", you can specify to docker that a file is a "docker file" but it is common sense to use the conventional name dockerfile (without extension)

There are some basic instructions that you use in the docker file: `FROM RUN CMD`, Let's see what are those and how to use them.

The very first instruction that the docker file starts with is `FROM`, and you have to give also the image you want to use (or use SCRATCH to pass no value) :

```
#USE scratch if you want an empty image
FROM ubuntu
```

Now we can add a maintainer, this part is optional, but it is best practice to give a maintainer of the image, so it will be more easy to find who is the maintainer of the image.  
You can give your Name and Surname and your email or just your email:

```
MAINTAINER simone panebianco <simone.panebianco@gmail.com>
```

Now that we have our basic image, let's say that we want to run something in the built container, for this we use the command `RUN`

```
RUN apt-get update
```

And if you want to run something on the command line you can use the `CMD` command:

```
 CMD ["echo","Hello World.."]
```

The `CMD` could be replaced in the `.yml` file using `command:`

**The difference beetween `CMD` and `RUN` is that `RUN` is executed during the building of the image, while the command you pass via `CMD` get executed only when you create a container out of the image.**

Now that we have our dockerfile, we can run it, to do so we use the `docker build` command

```
#if you are in the same folder of the dockerfile
docker build .
#if you are in a different folder of the dockerfile
docker build /PATH/TO/dockerfile
```

you can also name your image using the `-t` parmeter:

```
docker build -t myimage1:1.0
```

`myimage1` is the name while `1.0` is a tag that you give

Now that we have build our image (and that is saved in the docker image repository of our machine) we can do

```
docker images
```

To see the image that we have just created, and its ID, and since we have an image we can also do:

```
docker run id_image 
```

That will return us the famous "Hello world"
