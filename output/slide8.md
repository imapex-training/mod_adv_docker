
## Docker CLI: Accessing the Docker image shell

```
	docker run -it <your username>/imapex101_dockerfile /bin/bash

	# now you're in the container, type exit to stop the container
	root@80333d17d6c2:/# exit
```

* `-it` attaches to STDIN and creates a tty terminal to use
* `/bin/bash` specifies the command to run inside the container

