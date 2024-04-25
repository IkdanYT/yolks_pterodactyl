# Используем Python образ как базовый
FROM python:3.11-slim-bookworm

# Метаданные автора
LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Обновляем и устанавливаем необходимые пакеты как для Python, так и для Java.
RUN apt update \
    && apt -y install git gcc g++ ca-certificates dnsutils curl iproute2 ffmpeg procps tini fontconfig \
    && apt install -y curl lsof ca-certificates openssl git tar sqlite3 zip unzip \
    && apt clean

# Добавляем PPA для Temurin, чтобы установить Java
RUN mkdir -p /usr/share/man/man1 \
    && apt-get update -y \
    && apt-get install -y software-properties-common \
    && apt-get update -y \
    && apt-get install -y openjdk-8-jdk

# Создаем пользователя 'container'
RUN useradd -m -d /home/container container

USER container
ENV USER=container HOME=/home/container

WORKDIR /home/container

# Устанавливаем сигнал остановки
STOPSIGNAL SIGINT

# Копируем оба entrypoint файла
COPY --chown=container:container entrypoint-python.sh /entrypoint-python.sh
COPY --chown=container:container entrypoint-java.sh /entrypoint-java.sh

# Entry point и комманду CMD нам нужно будет объединять во время выполнения контейнера
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]

# Следующие строки - это пример, который запускает первый скрипт, а затем второй. 
# Это может быть не то, что вам нужно в зависимости от использования.
CMD ["/bin/bash", "-c", "/entrypoint-python.sh && /entrypoint-java.sh"]