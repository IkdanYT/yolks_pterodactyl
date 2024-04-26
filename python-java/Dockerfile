FROM python:3.11-slim-bookworm

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN apt update \
    && apt -y install git gcc g++ ca-certificates dnsutils curl iproute2 ffmpeg procps tini fontconfig \
    && apt install -y curl lsof ca-certificates openssl git tar sqlite3 zip unzip \
    && apt clean

RUN mkdir -p /usr/share/man/man1 \
    && apt-get update -y \
    && apt-get install -y software-properties-common \
    && apt-get update -y \
    && apt-get install -y openjdk-8-jdk

RUN useradd -m -d /home/container container

USER container
ENV USER=container HOME=/home/container

WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container entrypoint-python.sh /entrypoint-python.sh
COPY --chown=container:container entrypoint-java.sh /entrypoint-java.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]

CMD ["/bin/bash", "-c", "/entrypoint-python.sh && /entrypoint-java.sh"]