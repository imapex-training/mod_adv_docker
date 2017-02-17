
## Dockerfile: MAINTAINER and RUN
* `MAINTAINER` provides details about the author.  Providing your name and email are typical.  Email address within `< >` symbols.
* The next layer should install any packages needed.  Use a single `RUN` command and leverage `&&` to string commands together, and `\` to allow readabilty with wrapping

