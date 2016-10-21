
## A Simple Dockerfile

* Dockerfiles use `FROM` to indicate the base image

	```
	FROM debian:latest

	```
	
* the `:latest` part indicates the "tag" to use on the image.  Tags will be covered more later, but for now know that "latest" is a well understood standard tag in Docker.  If you do NOT explicitly set a tag, Docker will look for a "latest" tag.  


