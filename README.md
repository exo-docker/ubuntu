# exoplatform/ubuntu Docker image

Ubuntu container for eXo needs

| DOCKER IMAGE                    | UBUNTU VERSION |
| ------------------------------- | -------------- |
| *exoplatform/ubuntu*:**latest** | 18.04 LTS      |
| *exoplatform/ubuntu*:**v1804**  | 18.04 LTS      |
| *exoplatform/ubuntu*:**v1604**  | 16.04 LTS      |

## image content

* [Tini](https://github.com/krallin/tini) v0.18.0 - valid `init` for containers
* [gosu](https://github.com/tianon/gosu) v1.10 - sudo / su replacement for containers
* tooling : gpg, curl, wget, unzip, htop

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