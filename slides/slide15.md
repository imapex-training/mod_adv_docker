
## Adding an app

* Now we'll create a program to run in the container.  Run these commands to create a basic Hello World shell script.  

```
echo '#!/bin/bash' > hello_world.sh
echo "while [ 0 -eq 0 ]; do" >> hello_world.sh
echo "echo 'Hello World'" >> hello_world.sh
echo "sleep 2" >> hello_world.sh
echo "done" >> hello_world.sh
```

