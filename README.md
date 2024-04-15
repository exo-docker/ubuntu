# exoplatform/ubuntu Docker image <!-- omit in toc -->
[![Build & publish eXo Ubuntu images](https://github.com/exo-docker/ubuntu/actions/workflows/build.yml/badge.svg)](https://github.com/exo-docker/ubuntu/actions/workflows/build.yml)

Supported tags and respective `Dockerfile` links

| Ubuntu edition         | Docker tags             | Dockerfile                                 |
| ---------------------- | ----------------------- | ------------------------------------------ |
| Ubuntu Noble Numbat    | `24.04`, `24`, `latest` | [( 24.04/Dockerfile )](./22.04/Dockerfile) |
| Ubuntu Jammy Jellyfish | `22.04`, `22`           | [( 22.04/Dockerfile )](./22.04/Dockerfile) |
| Ubuntu Focal Fossa     | `20.04`, `20`           | [( 20.04/Dockerfile )](./20.04/Dockerfile) |
| Ubuntu Bionic Beaver   | `18.04`, `18`           | [( 18.04/Dockerfile )](./18.04/Dockerfile) |
| Ubuntu Xenial Xerus    | `16.04`, `16`           | [( 16.04/Dockerfile )](./16.04/Dockerfile) |


## image content

- [Tini](https://github.com/krallin/tini) v0.19.0 - valid `init` for containers ([more info](https://github.com/krallin/tini))

```Dockerfile
# the default entrypoint should remain :
ENTRYPOINT ["/usr/local/bin/tini", "--"]
# define in CMD your default image launch parameters
CMD ["/your/program", "-and", "-its", "arguments"]
```

- [gosu](https://github.com/tianon/gosu) v1.12 - sudo / su replacement for containers ([more info](https://github.com/tianon/gosu))

```bash
# gosu is a sudo and su replacement
# Usage: gosu user-spec command [args]

$ gosu tianon bash
$ gosu nobody:root bash -c 'whoami && id'
$ gosu 1000:1 id
```

- tooling : gpg, curl, wget, unzip, htop

- `wait-for-it.sh` utility script to test and wait on the availability of a TCP host and port (usefull to wait for another container availability) ([more info](https://github.com/vishnubob/wait-for-it))

```bash
#!/usr/bin/env bash
#
# wait for a mysql database availability during a maximum of 60 seconds
# and make the container startup failing if the 60 seconds timeout is reached
#
wait-for-it.sh my-db:3306 -s -t 60 || { echo "ERROR mysql database unavailable after 60s ! abort ..."; exit 1; }
```

- Ubuntu packages repositories

```txt
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs) main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-backports main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-security main restricted universe multiverse
```

## run

You can create a container :

```bash
# create a container and step inside with a shell
docker run --rm -ti exoplatform/ubuntu bash

# create a container and start htop
docker run --rm -ti exoplatform/ubuntu htop
```

## build

You can build the image with the following commands :

```bash
# get the sources
git co https://github.com/exo-docker/ubuntu.git
cd ./ubuntu/

# build the image
docker build -t exoplatform/ubuntu .
```
