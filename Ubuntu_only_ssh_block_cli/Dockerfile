FROM ubuntu:latest
# обновление репозиториев, установка пакетов и очистка apt кэша 
RUN apt update && \
    apt install openssh-server sudo nano vim -y && \
    rm -rf /var/lib/apt/lists/*
# создаём пользователя test, задаём пароли для остальных
RUN useradd -rm -d /home/test -s /bin/bash -g root -G sudo -u 999 test && \
    echo 'test:test' | chpasswd && \
    echo 'ubuntu:ubuntu' | chpasswd
# создаём каталог для службы sshd, задаём права на каталог
RUN mkdir -p -m0755 /var/run/sshd
CMD ["/usr/sbin/sshd","-D"]