# Dockerizing a base images with:
#
#   - Ubuntu 18.04
#
# Build:    docker build -t exoplatform/ubuntu .
#
# Run:      docker run -ti exoplatform/ubuntu

FROM  ubuntu:18.04
LABEL maintainer="eXo Platform <docker@exoplatform.com>"

ENV TINI_VERSION v0.18.0
ENV GOSU_VERSION 1.10

ENV TERM=xterm \
    DEBIAN_FRONTEND=noninteractive

WORKDIR /tmp

# Install base packages
# --force-confold: do not modify the current configuration file, the new version is installed with a .dpkg-dist suffix. With this option alone, even configuration
#   files that you have not modified are left untouched. You need to combine it with
# --force-confdef to let dpkg overwrite configuration files that you have not modified.
ENV _APT_OPTIONS -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold"
RUN apt-get -qq update \
  && apt-get -qq -y install ${_APT_OPTIONS} lsb-release \
  && echo "deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs) main restricted universe multiverse" > /etc/apt/sources.list \
  && echo "deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-updates main restricted universe multiverse" >> /etc/apt/sources.list \
  && echo "deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-backports main restricted universe multiverse" >> /etc/apt/sources.list \
  && echo "deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-security main restricted universe multiverse" >> /etc/apt/sources.list \
  && apt-get -qq update \
  && apt-get -qq -y upgrade ${_APT_OPTIONS} \
  && apt-get -qq -y install ${_APT_OPTIONS} \
    wget \
    curl \
    netcat \
    unzip \
    gpg \
    apt-transport-https \
    locales \
    expect \
    htop \
  && locale-gen en_US.UTF-8 \
  && apt-get -qq -y autoremove \
  && apt-get -qq -y clean \
  && rm -rf /var/lib/apt/lists/*

ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8

# Installing Tini
RUN set -ex \
    && ( \
        gpg --keyserver hkp://p80.pool.sks-keyservers.net:80    --recv-keys 595E85A6B1B4779EA4DAAEC70B588DFF0527A9B7 \
        || gpg --keyserver pgp.mit.edu                          --recv-keys 595E85A6B1B4779EA4DAAEC70B588DFF0527A9B7 \
        || gpg --keyserver keyserver.pgp.com                    --recv-keys 595E85A6B1B4779EA4DAAEC70B588DFF0527A9B7 \
    )

RUN set -ex \
    && wget -nv -O /usr/local/bin/tini https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini \
    && wget -nv -O /usr/local/bin/tini.asc https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini.asc \
    && gpg --verify /usr/local/bin/tini.asc \
    && chmod +x /usr/local/bin/tini

# Installing Gosu
RUN set -ex \
    && ( \
        gpg --keyserver hkp://p80.pool.sks-keyservers.net:80    --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
        || gpg --keyserver pgp.mit.edu                          --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
        || gpg --keyserver keyserver.pgp.com                    --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    )

RUN set -ex \
    && curl -sS -o /usr/local/bin/gosu -L "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
    && curl -sS -o /usr/local/bin/gosu.asc -L "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
    && gpg --verify /usr/local/bin/gosu.asc \
    && rm /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu

# Installing wait-for-it.sh utility
COPY bin/wait-for-it.sh /usr/local/bin/
RUN chown root:root /usr/local/bin/wait-for-it.sh \
    && chmod +x /usr/local/bin/wait-for-it.sh \
    && ln -s /usr/local/bin/wait-for-it.sh /usr/local/bin/wait-for.sh \
    && ln -s /usr/local/bin/wait-for-it.sh /usr/local/bin/wait-for

# Create needed directories
ENV DOWNLOAD_DIR /srv/downloads
RUN mkdir -p "${DOWNLOAD_DIR}" && chmod -R 777 "${DOWNLOAD_DIR}"

# Add some aliases
RUN echo "alias ll='ls -al --color'" > /etc/profile.d/aliases.sh \
    echo "alias rm='rm -i'" >> /etc/profile.d/aliases.sh

# Configure htop for root user
COPY conf/htoprc.conf /root/.config/htop/htoprc
RUN chown root:root /root/.config/htop/htoprc

ENTRYPOINT ["/usr/local/bin/tini", "--"]
CMD [ "bash" ]