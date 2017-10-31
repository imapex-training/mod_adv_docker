
[item]: # (slide)

![](http://imapex.io/images/imapex_standing_text_sm.png)

## Module: Advanced Docker

Learning about Docker, Docker Hub, and Containers

[item]: # (/slide)

## Abstract 

Along with micro-services, containerization of applications, particularly leveraging Docker, continues to be a major aspect of modern development practices and deployment strategies.  Docker makes it quite simple to pull down an application in an image and use it to start a new container.  Creating well designed and efficient custom image that are tagged and published to the proper registries as part of a code pipeline requires a bit more depth of knowledge.  Through the hands-on exercises in this module, we will walk through the most critical elements of leveraging Docker as a delivery mechanism for your code.  Topics discussed include Dockerfile creation, registry options, the meaning and use of tags, and auto-build repositories.  

### Presentation Slides

A slide show version of this presentation is available at this link: [Advanced Docker Slides](https://rawgit.com/imapex-training/mod_adv_docker/master/output/index.html#/)

[item]: # (slide)
# Main Topics

* [Dockerfiles](#section-1-dockerfiles)
* [Docker Registries](#section-2-registry-considerations)
* [Tags](#section-3-tags)
* [Manual vs Auto-Build Repos](#section-4-manual-vs-auto-build-repos)

[item]: # (/slide)

[item]: # (slide)
# Section 1: Dockerfiles
[item]: # (/slide)

A Docker container is built based on instructions that you write in a Dockerfile.  The Dockerfile begins with a source container image, and then each line in the file adds a new layer onto the starting point until the final container image is created.  Dockerfiles range from very simple, few line descriptions, to very complex files with many layers and configuration elements.  

As you work with Dockerfiles, and create containers for applications that others will use, it can be helpful to keep some best practices and standards in mind.  We'll walk through an example Dockerfile in the experiments section to demonstrate some best practices.  

# Why...

Containers, and Docker containers specifically, are rapidly becoming a common way to package and distribute applications.  No longer are installers, or binaries to download, considered an optimal way to deploy your application.  The ability to easily contain (no pun intended) and isolate everything needed for an application (and make it easy to standup in *any* container-friendly environment) makes it ideal for building demos and sample applications.  

[item]: # (slide)

##  Why Containers

* Easily package your application dependencies
* Deploy on many container-friendly platforms
* Container registries make it easy to distribute

[item]: # (/slide)

[item]: # (slide)

##  Docker Landscape

* Docker for Mac vs Docker Toolbox
* Docker Hub: Docker registry
* Docker Swarm: Clustering for Docker
* Docker is not the only game in town: rkt, LXC

[item]: # (/slide)

[item]: # (slide)

## Experiments

*In this section, use whatever text editor you wish when updating the Dockerfile*

[item]: # (/slide)

[item]: # (slide)

## Create a new directory for the Dockerfile practice

```
mkdir imapex101_dockerfile
cd imapex101_dockerfile
touch Dockerfile
```

[item]: # (/slide)

[item]: # (slide)

## A Simple Dockerfile

* Dockerfiles use `FROM` to indicate the base image

	```
	FROM debian:latest

	```

* the `:latest` part indicates the "tag" to use on the image.  Tags will be covered more later, but for now know that "latest" is a well understood standard tag in Docker.  If you do NOT explicitly set a tag, Docker will look for a "latest" tag.  


[item]: # (/slide)

[item]: # (slide)

## Simple Docker lifecycle

* Edit `Dockerfile`
* `docker build`
* `docker run`
* `GOTO 10`  `:-)`
* Whenever you edit your `Dockerfile`, you need to rebuild your container.

[item]: # (/slide)

[item]: # (slide)

## Docker CLI: Building a Docker image

* `docker build` is the command to build the container image.  When running use `-t <repositoryname>:<tag>` format to provide a repository name and tag for the image.  If you fail to specify, you can only refer to the image by the Image Id.
* Repository Names are in the format of `<username or org>/<repo-name>`.  This will be required if you plan to `push` your image to a registry.  

[item]: # (/slide)

[item]: # (slide)

* The last parameter is the location to find the Dockerfile to use.  Specifying `.` looks in the current directory for `Dockerfile`

```

$ docker build -t <your username>/imapex101_dockerfile:latest .

```

[item]: # (/slide)

[item]: # (slide)

### Example Output

```
	Sending build context to Docker daemon 2.048 kB
	Step 1 : FROM debian:latest
	latest: Pulling from library/debian
	357ea8c3d80b: Pull complete
	Digest: sha256:ffb60fdbc401b2a692eef8d04616fca15905dce259d1499d96521970ed0bec36
	Status: Downloaded newer image for debian:latest
	 ---> 1b01529cc499
	Successfully built 1b01529cc499
```

[item]: # (/slide)

[item]: # (slide)

* Run this command to view your newly created image

```
$ docker images

REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
<your username>/imapex101_dockerfile   latest              1b01529cc499        23 hours ago        125.1 MB

```

[item]: # (/slide)

[item]: # (slide)

## Docker CLI: Accessing the Docker image shell

```
	docker run -it <your username>/imapex101_dockerfile /bin/bash

	# now you're in the container, type exit to stop the container
	root@80333d17d6c2:/# exit
```

* `-it` attaches to STDIN and creates a tty terminal to use
* `/bin/bash` specifies the command to run inside the container

[item]: # (/slide)

[item]: # (slide)

## Docker CLI: Listing running or stopped containers

Run the `docker ps -a` command to see the remnants of your container... you could restart it from this state with docker start, but another docker run would create a new one

```
	$ docker ps -a
```

[item]: # (/slide)

[item]: # (slide)

### Example Output

```
	CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                     PORTS               NAMES
	80333d17d6c2        <your username>/imapex101_dockerfile   "/bin/bash"              35 seconds ago      Exited (0) 3 seconds ago                       nauseous_raman
```

[item]: # (slide)

## Dockerfile: MAINTAINER and RUN
* `MAINTAINER` provides details about the author.  Providing your name and email are typical.  Email address within `< >` symbols.
* The next layer should install any packages needed.  Use a single `RUN` command and leverage `&&` to string commands together, and `\` to allow readabilty with wrapping

[item]: # (/slide)

[item]: # (slide)

## Example

```
FROM debian:latest
MAINTAINER Your Name <email@domain.com>

# You can provide comments in Dockerfiles
# Install any needed packages for your application
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    mercurial \
 && rm -rf /var/lib/apt/lists/*
```

[item]: # (/slide)

[item]: # (slide)

* Now, build your image

```
docker build -t <your username>/imapex101_dockerfile:latest .
```

* Note the references to `Step 2` and `Step 3` and the `running in` comments.  Each command in the file, is a Step, and each step is a new layer.  Minimizing the number of layers created is always a goal in Dockerfile creation

[item]: # (/slide)

```
docker build -t <your username>/imapex101_dockerfile:latest .

Sending build context to Docker daemon 2.048 kB
Step 1 : FROM debian:latest
 ---> 1b01529cc499
Step 2 : MAINTAINER Hank Preston <hank.preston@gmail.com>
 ---> Using cache
 ---> 9f7054e99744
Step 3 : RUN apt-get update && apt-get install -y     aufs-tools     automake     build-essential     curl     dpkg-sig     mercurial  && rm -rf /var/lib/apt/lists/*
 ---> Running in 11f0aa8fea4c
Get:1 http://security.debian.org jessie/updates InRelease [63.1 kB]
Ign http://httpredir.debian.org jessie InRelease
Get:2 http://httpredir.debian.org jessie-updates InRelease [142 kB]
Get:3 http://httpredir.debian.org jessie Release.gpg [2373 B]
Get:4 http://security.debian.org jessie/updates/main amd64 Packages [371 kB]
Get:5 http://httpredir.debian.org jessie Release [148 kB]
Get:6 http://httpredir.debian.org jessie-updates/main amd64 Packages [17.6 kB]
Get:7 http://httpredir.debian.org jessie/main amd64 Packages [9032 kB]
Fetched 9776 kB in 18s (528 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
The following extra packages will be installed:
  autoconf autotools-dev binutils bzip2 ca-certificates cpp cpp-4.9 dpkg-dev
  fakeroot file...

  ####
  # output truncated
  ####

 ---> 331e39ad1dc4
Removing intermediate container 11f0aa8fea4c
Successfully built 331e39ad1dc4  
```

[item]: # (slide)
## Dockerfile: EXPOSE
* Use `EXPOSE` to specify what port(s) your container will use to host services externally.  For example, if you are building a Web App Container, you'll likely expose port 80.  

```
FROM debian:latest
MAINTAINER Your Name <email@domain.com>

# You can provide comments in Dockerfiles
# Install any needed packages for your application
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    mercurial \
 && rm -rf /var/lib/apt/lists/*

EXPOSE 80

```

[item]: # (/slide)

[item]: # (slide)

* Build a new version of the container
	* Note how the process takes much less time and the references to "Using cache".  Because there has been no change in the earlier steps, docker needn't rerun them
	* This is why proper consideration in the order of steps in your container creation is critical.  
	* Keep the steps likely to change (ie Copying in your code) to the end.  

[item]: # (/slide)

[item]: # (slide)

```
	docker build -t <your username>/imapex101_dockerfile:latest .

	Sending build context to Docker daemon 2.048 kB
	Step 1 : FROM debian:latest
	 ---> 1b01529cc499
	Step 2 : MAINTAINER Hank Preston <hank.preston@gmail.com>
	 ---> Using cache
	 ---> 9f7054e99744
	Step 3 : RUN apt-get update && apt-get install -y     aufs-tools     automake     build-essential     curl     dpkg-sig     mercurial  && rm -rf /var/lib/apt/lists/*
	 ---> Using cache
	 ---> 331e39ad1dc4
	Step 4 : EXPOSE 80
	 ---> Running in 67d60d70e80b
	 ---> 788c0d174e3d
	Removing intermediate container 67d60d70e80b
	Successfully built 788c0d174e3d
```

[item]: # (/slide)

[item]: # (slide)

## Adding an app

* Now we'll create a program to run in the container.  Run these commands to create a basic Hello World shell script.  

```
echo '#!/bin/bash' > hello_world.sh
echo "while [ 0 -eq 0 ]; do" >> hello_world.sh
echo "echo 'Hello World'" >> hello_world.sh
echo "sleep 2" >> hello_world.sh
echo "done" >> hello_world.sh
```

[item]: # (/slide)

[item]: # (slide)

## Dockerfile: COPY and CMD
* Use `COPY` to bring in your code to the container.  
* After copying your code into the container, use `RUN` commands to take whatever actions needed.  
* Use `CMD` to instruct the container what to do upon execution.  

[item]: # (/slide)

[item]: # (slide)

```
	FROM debian:latest
	MAINTAINER Your Name <email@domain.com>

	# You can provide comments in Dockerfiles
	# Install any needed packages for your application
	RUN apt-get update && apt-get install -y \
	    aufs-tools \
	    automake \
	    build-essential \
	    curl \
	    dpkg-sig \
	    mercurial \
	 && rm -rf /var/lib/apt/lists/*

	EXPOSE 80

	COPY hello_world.sh /root/
	RUN chmod +x /root/hello_world.sh

	CMD ["/root/hello_world.sh"]
```

[item]: # (/slide)

[item]: # (slide)

* Build a new version of the container

```
docker build -t <your username>/imapex101_dockerfile:latest .

Step 1 : FROM debian:latest
 ---> 1b01529cc499
Step 2 : MAINTAINER Hank Preston <hank.preston@gmail.com>
 ---> Using cache
 ---> 9f7054e99744
Step 3 : RUN apt-get update && apt-get install -y     aufs-tools     automake     build-essential     curl     dpkg-sig     mercurial  && rm -rf /var/lib/apt/lists/*
 ---> Using cache
 ---> 331e39ad1dc4
Step 4 : EXPOSE 80
 ---> Using cache
 ---> 788c0d174e3d
Step 5 : COPY hello_world.sh /root/
 ---> 93105ac34a3b
Removing intermediate container d973d85f74f2
Step 6 : RUN chmod +x /root/hello_world.sh
 ---> Running in 888ee3e0c9fd
 ---> 2524c6f6b0bb
Removing intermediate container 888ee3e0c9fd
Step 7 : CMD /root/hello_world.sh
 ---> Running in 38a8f37827db
 ---> afde9a78e299
Removing intermediate container 38a8f37827db
Successfully built afde9a78e299
```

[item]: # (/slide)

[item]: # (slide)
## Run container with app
* Run the final container version to execute the program.  We use `-d` to run our container in the background and print out the container id

```
docker run -d --name dockerfile <your username>/imapex101_dockerfile:latest

8b5b52eaa9a7c838c77bed791315a42ac7270e714c5fcd3ffbdbc49ef94b4316
```

[item]: # (/slide)

[item]: # (slide)

* Check out the status of the container with a few commands here

```
# Checkout the status of running containers here
docker ps

CONTAINER ID        IMAGE                                         COMMAND                  CREATED              STATUS              PORTS               NAMES
8b5b52eaa9a7        <your username>/imapex101_dockerfile:latest   "/root/hello_world.sh"   About a minute ago   Up About a minute   80/tcp              dockerfile
```

[item]: # (/slide)

[item]: # (slide)

## Docker CLI: docker logs

```
docker logs dockerfile

Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
```

*  Sure enough, it's hello worlding away

[item]: # (/slide)

[item]: # (slide)

*  you can stop and start the same container like this

```
$ docker stop dockerfile
```

[item]: # (/slide)

[item]: # (slide)

* Look at all docker containers and see its stopped

```
$ docker ps -a

CONTAINER ID        IMAGE                                         COMMAND                  CREATED             STATUS                        PORTS               NAMES
8b5b52eaa9a7        <your username>/imapex101_dockerfile:latest   "/root/hello_world.sh"   4 minutes ago       Exited (137) 14 seconds ago                       dockerfile
```

[item]: # (/slide)

[item]: # (slide)

* Start it back up

```
$ docker start dockerfile

$ docker ps

CONTAINER ID        IMAGE                                         COMMAND                  CREATED             STATUS              PORTS               NAMES
8b5b52eaa9a7        <your username>/imapex101_dockerfile:latest   "/root/hello_world.sh"   5 minutes ago       Up 2 seconds        80/tcp              dockerfile

```

[item]: # (/slide)

[item]: # (slide)

## Links

The experiments were just a walkthrough of the very basics.  Review these links for a more thorough discussion on best practices, and for examples of good Dockerfiles.  

* [https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)
* [https://github.com/docker-library/golang/blob/7fd2b76513e537343f088da671a51f5b2ea7d4c3/1.5/Dockerfile](https://github.com/docker-library/golang/blob/7fd2b76513e537343f088da671a51f5b2ea7d4c3/1.5/Dockerfile)
* [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
* [http://zeroturnaround.com/wp-content/uploads/2016/03/Docker-cheat-sheet-by-RebelLabs.png](http://zeroturnaround.com/wp-content/uploads/2016/03/Docker-cheat-sheet-by-RebelLabs.png)

[item]: # (/slide)

[item]: # (slide)
## Why do we care

We'll be creating docker containers for many of our demos.  Though we don't need the level of exactness used in commercial distributions, we do want to make sure we build respectable containers, that follow standards and best practices.  Also, doing what we can to keep them small and quick to build/rebuild will help in our development practices.  

[item]: # (/slide)

[item]: # (slide)
## Go Do it Exercises

For this exercise, build a new Dockerfile that...

* Uses an "alpine" image (you'll need to find out what that means)
* Runs [dbarnett/python-helloworld](https://github.com/dbarnett/python-helloworld) when started
[item]: # (/slide)
---------------------------------------

[item]: # (slide)
# Section 2: Registry Considerations
[item]: # (/slide)

With Docker, "registry" refers to a server that stores and hosts container images for centralized distribution.  [hub.docker.com](https://hub.docker.com) is the public, default registry for Docker images, but it is not the only option to be aware of.  

When you run a `docker pull` and grab an image from hub.docker.com, you are putting your trust in whoever maintains the image you are downloading.  The "official images" *(those images referred to simply by a name like `docker pull debian` rather than with an org or user such as `docker pull acme\debian`)* go through some level of validation and security testing by the community, but the vast majority of images available on hub.docker.com should be carefully considered before being used.  

Most enterprises will bring up "Trusted Registries" which is really just a fancy word for a private registry, installed and managed by the enterprise themself, where all images that are available have been vetted to be secure and stable.  

There are several options for enterprises looking for a private registry solution.  Actually, there are several alternatives to hub.docker.com for public registies as well.  

Some other companies that provide registries are

[item]: # (slide)

## Docker Registries

* Docker
	* Public: [hub.docker.com](https://hub.docker.com)
	* Enterprise Private: [Docker Trusted Registry](https://docs.docker.com/docker-trusted-registry/)
	* Free Private: [registry](https://hub.docker.com/_/registry)
* CoreOS
	* Support for both Docker and their container technology rckt
	* Public: [Quay](https://quay.io)
	* Private: [Quay Enterprise](https://quay.io)
* Google Container Registry
	* Public: [Google Container Registry](https://cloud.google.com/container-registry/)
* Amazon EC2 Container Registry
	* Public: [EC2 Container Registry](https://aws.amazon.com/ecr/)

[item]: # (/slide)

When leveraging a registry other than hub.docker.com, you must include it in all comands and container names.  For example, `docker pull quay.io/acme/demo:latest` would use the registry `quay.io`, and access the container `demo`, and tag `latest` from user `acme`.  

[item]: # (slide)

## Experiments

Let's see what it looks like to use a registry other than hub.docker.com.  We'll be using the image [quay.io/repository/hpreston/demo](https://quay.io/repository/hpreston/demo)

[item]: # (/slide)

[item]: # (slide)

* Pull down the image

```
docker pull quay.io/hpreston/demo:latest

docker images
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
quay.io/hpreston/demo           latest              afde9a78e299        58 minutes ago      371.3 MB

```

[item]: # (/slide)

[item]: # (slide)

* Run a container based on the image

```
docker run -d --name quaydemo quay.io/hpreston/demo:latest

docker ps
CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS               NAMES
23a23fcb3d26        quay.io/hpreston/demo:latest   "/root/hello_world.sh"   3 seconds ago       Up 2 seconds        80/tcp              quaydemo
```

[item]: # (/slide)

[item]: # (slide)

* Check the logs

```
docker logs quaydemo

Hello World
Hello World
Hello World
Hello World
```

[item]: # (/slide)

[item]: # (slide)
## Links

* [https://mesosphere.com/blog/2015/10/14/docker-registries-the-good-the-bad-the-ugly/](https://mesosphere.com/blog/2015/10/14/docker-registries-the-good-the-bad-the-ugly/)
* [https://aws.amazon.com/ecr/](https://aws.amazon.com/ecr/)
* [https://docs.docker.com/docker-trusted-registry/](https://docs.docker.com/docker-trusted-registry/)
* [https://hub.docker.com/_/registry/](https://hub.docker.com/_/registry/)
* [https://quay.io](https://quay.io)
* [Google Container Registry](https://cloud.google.com/container-registry/)

[item]: # (/slide)

[item]: # (slide)
## Why do we care

Using the basic public registry from docker, or other public registries are fine for learning and experimentation.  But enterprises of any size, will be looking for options for trusted registries.  Understanding the options available, and how to work with them is important.  

[item]: # (/slide)

[item]: # (slide)

## Go Do it Exercises

* Create a free account on Quay.io
* Create a public registry called demo and push up the sample Docker container created above.  
* Hint... you'll need to work out how to login using the docker cli

[item]: # (/slide)
---------------------------------

[item]: # (slide)
# Section 3: Tags
[item]: # (/slide)

[item]: # (slide)
There is a hierarchy of objects within Docker that are worth having a solid understanding of.

* Registry
	* User/Org/Account
		* Repository
			* Tagged Image

[item]: # (/slide)

[item]: # (slide)

What we commonly think of as a Docker container would better be described as a _specifically **tagged image**, contained within a **repository**, managed by a **user or organization**, hosted on a particular **registry**_

To be accurate, *container* is actually an instance of a tagged image that is running, or is currently stopped.  

[item]: # (/slide)

When getting started with Docker, it is easy, and perfectly fine to slip over the details, but if you are going to be actively building software to be distributed with containers, it's critical to understand the actual objects.  

[item]: # (slide)
Within a single Repository, tags are used to provide different versions of what is typically the same application or service.  Some common uses of the tags would be:

* Actual version control
	* It is common to see tags like **:v0.1**, **:v0.2**, **:v0.3**
* Variations of the base image
	* **python:2-alpine**, **python:2-wheezy**, **python:2-slim**
* Stages of Development
	* **:dev**, **:prod**, **:qa**
* Indications of Git Branches/Tags
	* **:feature/test**, **:feature/ui**, **:master**

[item]: # (/slide)

[item]: # (slide)

The tag **:latest** is a well understood standard in Docker to indicate a "default" image.  If you do not specify a particular tag when building or pulling, latest will be assumed.  It is up to the owner of the repository to manage what image is provided by **:latest**, and in fact it is possible to have a repository lacking a **:latest** tag.  That is considered bad practice and should be avoided.  Also remember that **:latest** rarely actually indicates the most recently built container... 	

[item]: # (/slide)

[item]: # (slide)

## Experiments
[item]: # (/slide)

[item]: # (slide)

* Pull down the default (aka latest) python image

```
docker pull python

Using default tag: latest
latest: Pulling from library/python
5c90d4a2d1a8: Pull complete
ab30c63719b1: Pull complete
c6072700a242: Pull complete
abb742d515b4: Pull complete
7663bd2e167e: Pull complete
d0f49e193c5a: Pull complete
28969a7d8e03: Pull complete
Digest: sha256:27cae89ba088e51a5c169f019ca6760d24a4e01c8453052631803a4785eda781
Status: Downloaded newer image for python:latest

docker images

REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
python                          latest              9152ad50a7f9        9 days ago          694.3 MB
```

[item]: # (/slide)

[item]: # (slide)

* As expected, we pulled down `python:latest`.  It's a very large image, 694.3 MBs.  That's fine for playing around, but for an actual container deployment, that's laughably large.  Let's pull down a couple of other python images that are better for actual usage.  
	* **:slim** is a tag on python that provides a "skinnied" down version of the image
	* **:alpine** is an image based on alpine linux.  A specific distribution designed for compactness for use in containers.

[item]: # (/slide)

[item]: # (slide)

```
docker pull python:slim
docker pull python:alpine

docker images

REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
python                          alpine              0298711f3095        9 days ago          73.04 MB
python                          slim                bdec2259b96a        9 days ago          221.3 MB
python                          latest              9152ad50a7f9        9 days ago          694.3 MB

```

[item]: # (/slide)

[item]: # (slide)

* Let's use our sample Dockerfile and create some new tagged images

```
docker build -t <your username>/imapex101_dockerfile:fun .
docker build -t <your username>/imapex101_dockerfile:in .
docker build -t <your username>/imapex101_dockerfile:the .
docker build -t <your username>/imapex101_dockerfile:sun .

docker images

REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
<your username>/imapex101_dockerfile   fun                 b6578662ca26        34 minutes ago      371.3 MB
<your username>/imapex101_dockerfile   in                  b6578662ca26        34 minutes ago      371.3 MB
<your username>/imapex101_dockerfile   latest              b6578662ca26        34 minutes ago      371.3 MB
<your username>/imapex101_dockerfile   sun                 b6578662ca26        34 minutes ago      371.3 MB
<your username>/imapex101_dockerfile   the                 b6578662ca26        34 minutes ago      371.3 MB

```

[item]: # (/slide)

[item]: # (slide)

* Note how each of the tags, actually are the same IMAGE ID.  This is because Docker is smart enough to realize they are the exact same image, and being efficient, it just reuses the same underlying image.  
* Having the same image, with multiple tags is a common aspect of docker.  

[item]: # (/slide)

[item]: # (slide)

## Links

* [https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375#.2gilq1dnl](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375#.2gilq1dnl)
* [http://www.carlboettiger.info/2014/08/29/docker-notes.html](http://www.carlboettiger.info/2014/08/29/docker-notes.html)

[item]: # (/slide)

[item]: # (slide)

## Why do we care

To really use Docker effectively, you must understand the concept of tagging.  Not only to make sure you are consuming containers as you intend, but also so you can setup your own repositories appropriately and as consumers will expect.  

[item]: # (/slide)

[item]: # (slide)

## Go Do it Exercises

* Look at some commonly used repositories and see how they leverage tags.  Some examples to check are:
	* MySQL
	* CentOS
	* Debian
	* nginx
	* Wordpress
* Pull down CentOS images for version 6 and 7 and start containers based on each
* Think about how you'll use tags with your demo applications... be prepared to discuss and defend your ideas

[item]: # (/slide)
--------------------------------------------------------

[item]: # (slide)
# Section 4: Manual vs Auto-Build Repos
[item]: # (/slide)

So far, we've been building our images locally on our laptops, or downloading them from repositories.  This is great for testing and learning, but it isn't practical for actual projects and applications.  The goal of DevOps is to automate all the little things, like building containers, and pushing them to registries.  For our projects in imapex, there are two methods we'll look at for building and updating repos.  

The simplest solution are Automated Build repos.  These are repositories that are created and linked to a source code repository such as GitHub.  They leverage WebHooks to be notified anytime there is an event like a code commit, branch update, or pull request.  When these events happen, the registry (ie Docker Hub) will clone the code and attempt to build a new image based on the Dockerfile.  As long as the build completes successfully, it will publish a new image to the repository.  This solution is easy to setup, and requires nothing outside of a GitHub and Docker Hub account, but lacks methods for testing or validation of the changed code in the repository.  

The second option is to rely on some other mechanism to track code changes, build new images, and publish them to the registry.  These tasks, and more, are the purpose of a CICD tool.  CICD is covered in Module 4.  

[item]: # (slide)
## Experiments

* Walk through creating a GitHub repo for the dockerfile example in this module
* Create an automated build repo on docker hub
* Update the code, commit and push to GitHub and watch what Docker does

[item]: # (/slide)

[item]: # (slide)
## Links

* [https://docs.docker.com/docker-hub/builds/](https://docs.docker.com/docker-hub/builds/)
[item]: # (/slide)
[item]: # (slide)
## Why do we care

As we build our applications and demos, we'll need a way to easily make the updates and changes available for distribution.  Though we could manually build, and push from our laptops during development, these are manual steps that take time away from the actual coding.  Further... no one actually building applications works that manually.  We need to be operating like developers.  
[item]: # (/slide)
[item]: # (slide)
## Go Do it Exercises

Automated builds are not just available for Docker Hub, Quay.io offers them as well.  Create an automated build on Quay.io for your GitHub project.  
[item]: # (/slide)
