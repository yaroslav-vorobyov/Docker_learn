1. [Первоисточник](https://github.com/stephenc/busybox-httpd-docker)
1. [Docker Docs - docker cp](https://docs.docker.com/reference/cli/docker/container/cp/)


### Сборка и запуск контейнера:

[Сборка по аналогии (ключи)](https://github.com/yaroslav-vorobyov/Docker_learn/blob/main/Ubuntu_only_ssh_block_cli/README.md#%D1%81%D0%B1%D0%BE%D1%80%D0%BA%D0%B0-%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%B0-%D0%B8%D0%B7-dockerfile-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B5%D1%82%D1%81%D1%8F-%D1%81%D0%BE%D0%BA%D1%80%D0%B0%D1%89%D0%B5%D0%BD%D0%BD%D0%B0%D1%8F-%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F)

    docker image build --tag busybox_httpd:latest . 

[Запуск контейнера по аналогии (ключи)](https://github.com/yaroslav-vorobyov/Docker_learn/blob/main/Ubuntu_only_ssh_block_cli/README.md#%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B0)

    docker container run --detach --interactive --tty --name <container_name> --publish 8080:80 <image_name>

### Изменение index.html:

> * стандартное приветствие ;)

    docker exec <container_name | ID> sh -c "echo '<h1>Hello World!</h1>' > /var/www/html/index.html"

> * нестандартное приветствие 8D
> 
> где:
> * **$(hostname)** - имя докерхоста
> * в подфункции забирается значение переменной HOME из контейнера 

    docker exec <container_name | ID> sh -c "echo '<h1>Dockerhost is $(hostname)<br>Homedir in container is $(docker exec busybox env | grep -i HOME | awk -F '=' '{print $2}')</h1>' > /var/www/html/index.html"

### Выгрузка файла из контейнера:

    docker container cp <container_name | ID>:/var/www/html/index.html ./

### Прибраться:

[Прибраться по аналогии (ключи)](https://github.com/yaroslav-vorobyov/Docker_learn/blob/main/Ubuntu_only_ssh_block_cli/README.md#%D0%BF%D1%80%D0%B8%D0%B1%D1%80%D0%B0%D1%82%D1%8C%D1%81%D1%8F)

    docker container stop --time 2 <container_name | ID> && docker container prune -f && docker image rm <image_name | ID>