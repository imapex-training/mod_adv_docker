
## Dockerfile: COPY and CMD
* Use `COPY` to bring in your code to the container.  
* After copying your code into the container, use `RUN` commands to take whatever actions needed.  
* Use `CMD` to instruct the container what to do upon execution.  

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
	
