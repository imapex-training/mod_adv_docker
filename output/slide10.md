
## Docker CLI: Building a Docker image

* `docker build` is the command to build the container image.  When running use `-t <repositoryname>:<tag>` format to provide a repository name and tag for the image.  If you fail to specify, you can only refer to the image by the Image Id.
* Repository Names are in the format of `<username or org>/<repo-name>`.  This will be required if you plan to `push` your image to a registry.  

