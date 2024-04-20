FROM ubuntu:latest

# указание имени разработчика
# MAINTAINER (deprecated) name <mail@example.com>
LABEL org.opencontainers.image.authors="mail@mail.ru"

# указание команд которые будут выполнены внутри образа
RUN apt-get update && apt-get install -y cowsay && ln -s /usr/games/cowsay usr/bin/cowsay

# определить запускаемый файл при запуске docker run
ENTRYPOINT ["cowsay"]