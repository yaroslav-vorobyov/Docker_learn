1. [Docker Legacy Docs - Dockerize an SSH service](https://docker-docs.uclv.cu/engine/examples/running_ssh_service/)
2. [Docker Docs - Docker Context](https://docs.docker.com/engine/context/working-with-contexts/)
3. [Docker Docs - update context](https://docs.docker.com/engine/context/working-with-contexts/#updating-a-context)
4. [Docker Docs - SSH protect the Docker daemon socket](https://docs.docker.com/engine/security/protect-access/#use-ssh-to-protect-the-docker-daemon-socket)
5. [Docker Docs - Docker Run](https://docs.docker.com/reference/cli/docker/container/run/)
6. [Docker Docs - container ls --no-trunc](https://docs.docker.com/reference/cli/docker/container/ls/#no-trunc)
7. [Docker Docs - container ls --filter 'ancestor...='](https://docs.docker.com/reference/cli/docker/container/ls/#ancestor)
8. [Docker Docs - container ls --format '{{ }}'](https://docs.docker.com/reference/cli/docker/container/ls/#format)
9. [Docker Docs - container inspect](https://docs.docker.com/reference/cli/docker/inspect/)
10. [Docker Docs - container port](https://docs.docker.com/reference/cli/docker/container/port/)

### Сборка образа из Dockerfile (используется сокращенная версия):

> * **--tag** - индивидуальная метка для нового образа \<IMAGE : TAG>

    docker image build --tag <image_name:tag_name> .

### Запуск контейнера:

> * **--detach** - запуск в фоне
> * **--interactive** - с интерактивной консолью STDIN
> * **--tty** - с поддержкой интерактивного STDIN и STDOUT
> * **--name** - имя контейнера
> * **--publish** - проброс портов (локально:внутри)

    docker container run --detach --interactive --tty --name <container_name> --publish 2222:22 <image_name:tag_name>

### Вывод ID контейнера(-ов):

> * **--all** - вывести все контейнеры
> * **--format** - вывести информацию о контейнерах в соответствии с шаблоном
> * **--filter** - отфильтровать контейнеры наследующие определенный образ
> * **--no-trunc** - вывести информацию без усечения

    docker container ls --all --format '{{.ID}}' --filter 'ancestor=ubuntu:ssh' --no-trunc

### Вывод IP-адреса контейнера(-ов)
    
> * в подфункции забираются все ID контейнеров, отфильтрованных по определённому наследуемому образу

    docker container inspect $(docker container ls --all --format '{{.ID}}' --filter 'ancestor=ubuntu:ssh') --format '{{.NetworkSettings.IPAddress}}'

### Вывод занимаемого(-ых) порта(-ов) контейнером(-ами)

> * в подфункции забираются все ID контейнеров без усечения, отфильтрованных по определённому наследуемому образу

    docker container port $(docker container ls --all --format '{{.ID}}' --filter 'ancestor=ubuntu:ssh' --no-trunc)

### Подключение к контейнеру по SSH

    ssh test@localhost -p 2222

### Ограничение подключения (конкретный пользователь по SSH):

> **`** - символ конца каретки (escape character pwsh)

    docker context create `
    --docker host=ssh://test@$(docker container ls --all --format '{{.ID}}' --filter 'ancestor=ubuntu:ssh' --no-trunc) `
    --description=<"Desciption"> `
    <context_name>

### Все контекты в системе:

    docker context list

### Обновить контекст (строку подключения, параметр --docker):

    docker context update <context_name> --docker "host=ssh://test@localhost"

### Переключиться на контекст:

> * стандартный (включен по умолчанию, как активный отмечен `*`)

    docker context use default

> * свой (при переключении будет отмечен `*`)

    docker context use <context_name>

### Прибраться:

> * **--time** - сократить время ожидания до остановки контейнера до 2 сек (вместо стандартных 10); использовать только, если есть уверенность, что контейнер завершит внутренние операции перед получением им сигнала SIGKILL за установленное время.

    docker container stop --time 2 my_container && docker container prune -f && docker image rm ubuntu:ssh
    docker context rm <context_name>