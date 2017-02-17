
* Now, build your image

```
docker build -t <your username>/imapex101_dockerfile:latest .
```

* Note the references to `Step 2` and `Step 3` and the `running in` comments.  Each command in the file, is a Step, and each step is a new layer.  Minimizing the number of layers created is always a goal in Dockerfile creation

