---

title: Dorcker

date: 2019-08-28 21:23:39
tags: [How]
categories: [Tutorial]

---


<!-- vim-markdown-toc GFM -->

* [Install](#install)
    * [nvidia-docker](#nvidia-docker)
* [What](#what)
    * [Dockerfile](#dockerfile)
    * [Containers](#containers)
* [Command](#command)
* [Tips](#tips)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Install

[官网](https://docs.docker.com/install/linux/docker-ce/debian/)

命令:

```shell
wget -qO- https://get.docker.com/ | sh
sudo usermod -aG docker $USER
newgrp docker
```

## nvidia-docker

[官网](https://nvidia.github.io/nvidia-docker/)

命令:

```shell
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
         sudo apt-key add -

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
         sudo tee /etc/apt/sources.list.d/nvidia-docker.list
         sudo apt-get update

sudo apt-get install nvidia-docker2
sudo pkill -SIGHUP dockerd
sudo /etc/init.d/docker restart
```

-----------------------------------------------------------------

# What

## Dockerfile

> A Dockerfile is a file that you create which in turn produces a Docker image when you build it.
>
> A Dockerfile is a __recipe__ for building Docker images, and the act of running
> a separate build command produces the Docker image from that __recipe__.
>
> In the world of object oriented programming, you often deal with classes. You can think of a Docker
> image as a class, where as a Docker container is an instance of that class. [^dockerfile]

**将docker image比作class, container比作instance不准确, 但足够理解**

[^dockerfile]: https://nickjanetakis.com/blog/differences-between-a-dockerfile-docker-image-and-docker-container

## Containers

> A Docker container is just a process / service that runs directly on your machine. It is slightly
different than a regular process because the Docker daemon along with the Linux kernel do a few things to
ensure it runs in total isolation.
>
> A virtual machine is typically used to isolate an entire system. [^container]

[^container]: https://nickjanetakis.com/blog/docker-tip-1-docker-containers-are-isolated-processes-not-virtual-machines

-----------------------------------------------------------------

# Command

pass

-----------------------------------------------------------------

# Tips

pass

-----------------------------------------------------------------

# References

- <https://nickjanetakis.com/blog/tag/docker-tips-tricks-and-tutorials>
