# Используем Python образ как базовый
FROM python:3.11-slim-bookworm

# Метаданные автора
LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Обновляем и устанавливаем зависимости для Python и Java в одном RUN блоке
RUN apt-get update -y -qq \
    && apt-get install -y -qq software-properties-common curl lsof ca-certificates openssl git tar sqlite3 zip unzip fontconfig \
    && apt-get install -y -qq openjdk-8-jdk --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Добавьте вывод ошибок для apt-get чтобы видеть потенциальные ошибки в процессе сборки
RUN apt-get update -y \
    && apt-get -o Debug::pkgProblemResolver=yes -f install

# Создаем пользователя 'container'
RUN useradd -m -d /home/container -s /bin/bash container

USER container
ENV USER=container HOME=/home/container

WORKDIR /home/container

# Устанавливаем сигнал остановки
STOPSIGNAL SIGINT

# Копируем файлы entrypoint
COPY --chown=container:container entrypoint-python.sh /entrypoint-python.sh
COPY --chown=container:container entrypoint-java.sh /entrypoint-java.sh

# Делаем их исполняемыми
RUN chmod +x /entrypoint-python.sh /entrypoint-java.sh

# Устанавливаем entrypoint и cmd
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/bin/bash", "-c", "/entrypoint-python.sh && /entrypoint-java.sh"]