
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

