# Используем Python образ как базовый
FROM python:3.11-slim-bookworm

# Метаданные автора
LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Обновляем и устанавливаем необходимые пакеты как для Python, так и для Java.
RUN apt-get update -y && apt-get install -y \
    git \
    gcc \
    g++ \
    ca-certificates \
    dnsutils \
    curl \
    iproute2 \
    ffmpeg \
    procps \
    tini \
    fontconfig \
    lsof \
    openssl \
    tar \
    sqlite3 \
    zip \
    unzip \
    default-jdk \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

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

# Даем разрешения на исполнение entrypoint скриптам
RUN chmod +x /entrypoint-python.sh && chmod +x /entrypoint-java.sh

# Entry point и комманду CMD нам нужно будет объединять во время выполнения контейнера
ENTRYPOINT ["/usr/bin/tini", "--"]

# Следующие строки - это пример, который запускает первый скрипт, а затем второй. 
# Это можно адаптировать в зависимости от потребностей.
CMD ["/bin/bash", "-c", "/entrypoint-python.sh && /entrypoint-java.sh"]