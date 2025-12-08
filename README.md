# exoplatform/ubuntu Docker image <!-- omit in toc -->

[![Build & publish eXo Ubuntu images](https://github.com/exo-docker/ubuntu/actions/workflows/build.yml/badge.svg)](https://github.com/exo-docker/ubuntu/actions/workflows/build.yml)

## Supported tags and respective `Dockerfile` links

| Ubuntu edition                         | Docker tags             | Dockerfile                                   |
| -------------------------------------- | ----------------------- | -------------------------------------------- |
| Ubuntu 24.04 (latest LTS)              | `24.04`, `24`, `latest` | [(24.04/Dockerfile)](./24.04/Dockerfile)     |
| Ubuntu Rolling (latest normal release) | `rolling`               | [(rolling/Dockerfile)](./rolling/Dockerfile) |
| Ubuntu Jammy Jellyfish (22.04 LTS)     | `22.04`, `22`           | [(22.04/Dockerfile)](./22.04/Dockerfile)     |
| Ubuntu Focal Fossa (20.04 LTS, EOL)    | `20.04`, `20`           | [(20.04/Dockerfile)](./20.04/Dockerfile)     |

> **Note:** `latest` points to the most recent LTS (24.04). `rolling` tracks the newest Ubuntu release (non-LTS or LTS).

## Image content

* **[Tini](https://github.com/krallin/tini) v0.19.0** – minimal init for containers

```dockerfile
# Default entrypoint
ENTRYPOINT ["/usr/local/bin/tini", "--"]
# Default CMD
CMD ["/your/program", "-and", "-its", "arguments"]
```

* **[Gosu](https://github.com/tianon/gosu) v1.12** – sudo/su replacement for containers

```bash
# Usage examples
gosu tianon bash
gosu nobody:root bash -c 'whoami && id'
gosu 1000:1 id
```

* Tooling included: `gpg`, `curl`, `wget`, `unzip`, `htop`
* `wait-for-it.sh` script to wait for TCP host/port availability ([more info](https://github.com/vishnubob/wait-for-it))

```bash
#!/usr/bin/env bash
# wait for a MySQL database (max 60 seconds)
wait-for-it.sh my-db:3306 -s -t 60 || { echo "ERROR mysql database unavailable after 60s! Abort."; exit 1; }
```

* Ubuntu package repositories:

```txt
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs) main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-backports main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-security main restricted universe multiverse
```

## Run

```bash
# Start a container with an interactive shell
docker run --rm -ti exoplatform/ubuntu bash

# Start a container and run htop
docker run --rm -ti exoplatform/ubuntu htop
```

## Build

```bash
# Get the sources
git clone https://github.com/exo-docker/ubuntu.git
cd ./24.04/

# Build the image
docker build -t exoplatform/ubuntu .
```
