# exoplatform/ubuntu Docker image

Ubuntu container for eXo needs

| DOCKER IMAGE                    | UBUNTU VERSION            |
| ------------------------------- | ------------------------- |
| *exoplatform/ubuntu*:**latest** | 18.04 LTS (Bionic Beaver) |
| *exoplatform/ubuntu*:**v1804**  | 18.04 LTS (Bionic Beaver) |
| *exoplatform/ubuntu*:**v1604**  | 16.04 LTS (Xenial Xerus)  |

## image content

* [Tini](https://github.com/krallin/tini) v0.18.0 - valid `init` for containers
* [gosu](https://github.com/tianon/gosu) v1.10 - sudo / su replacement for containers
* tooling : gpg, curl, wget, unzip, htop

```txt
deb http://archive.ubuntu.com/ubuntu bionic main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu bionic-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu bionic-backports main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu bionic-security main restricted universe multiverse
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