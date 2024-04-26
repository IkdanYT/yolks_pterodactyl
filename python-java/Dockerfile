# Используем Python образ как базовый
FROM python:3.11-slim

# Метаданные автора
LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Обновим пакеты и установим зависимости
# Также добавим '-o APT::Get::Show-Upgraded=true' для вывода информации о пакетах, которые будут обновлены
RUN apt-get update -y -o APT::Get::Show-Upgraded=true && apt-get install -y -qq --no-install-recommends \
    software-properties-common \
    curl \
    lsof \
    ca-certificates \
    openssl \
    git \
    tar \
    sqlite3 \
    zip \
    unzip \
    fontconfig \
    && rm -rf /var/lib/apt/lists/*

# Добавление репозитория Temurin и установка JDK
RUN apt-get update -y -o APT::Get::Show-Upgraded=true && \
    mkdir -p /usr/share/man/man1 && \
    apt-get install -y -qq --no-install-recommends gnupg && \
    echo "deb http://ppa.launchpad.net/eclipse-team/temurin/ubuntu focal main" | tee /etc/apt/sources.list.d/temurin.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886 && \
    apt-get update -y -o APT::Get::Show-Upgraded=true && apt-get install -y -qq openjdk-8-jdk --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Создание пользователя
RUN useradd -ms /bin/bash container
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# Копирование и установка прав на entrypoints
COPY --chown=container:container entrypoint-python.sh /entrypoint-python.sh
COPY --chown=container:container entrypoint-java.sh /entrypoint-java.sh
RUN chmod +x /entrypoint-python.sh /entrypoint-java.sh

STOPSIGNAL SIGINT

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/bin/bash", "-c", "./entrypoint-python.sh && ./entrypoint-java.sh"]