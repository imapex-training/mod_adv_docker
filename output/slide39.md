
* Run a container based on the image

```
docker run -d --name quaydemo quay.io/hpreston/demo:latest

docker ps
CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS               NAMES
23a23fcb3d26        quay.io/hpreston/demo:latest   "/root/hello_world.sh"   3 seconds ago       Up 2 seconds        80/tcp              quaydemo
```

