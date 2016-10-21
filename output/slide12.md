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
