
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

