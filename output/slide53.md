
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

