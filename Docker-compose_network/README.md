1. [Docker Docs - Network](https://docs.docker.com/reference/cli/docker/network/create/)
2. [Docker Docs - --detach](https://docs.docker.com/reference/cli/docker/container/run/#detach)
3. [Docker Docs - Volumes](https://docs.docker.com/storage/volumes/#start-a-container-with-a-volume)
4. [Docker Docs - Compose](https://docs.docker.com/reference/cli/docker/compose/)
4. [Docker Docs - Compose Name-top-level](https://docs.docker.com/compose/compose-file/04-version-and-name/#name-top-level-element)
<br>

### Создать свою подсеть со узнаваемым именем

    # --driver - тип коммутирующего "устройства", bridge - по умолчанию (можно не задавать)
    # --attachable - снять ограничения для присоединения контейнеров к overlay network, запущенных вручную

    docker network create --driver bridge --attachable <ext_network_name>
<br>

### Удалить свою подсеть с узнаваемым именем
    
    docker network rm <ext_network_name>
<br>

### Посмотреть список подсетей

    docker network ls
<br>

### Посмотреть список запущенных контейнеров в своей подсети
    
    # имя сети задаётся 

    docker network inspect <ext_network.name>
<br>

### Запуск MariaDB в своей подсети (CLI)
    ##  пароли видны в явно открытом виде!!!
    # --detach - запуск в фоне
    # --name - задаём имя контейнера
    # --env MARIADB_USER, --env MARIADB_PASSWORD - задаём имя и пароль непривилегированного пользователя
    # --env MARIADB_ROOT_PASSWORD - задаём пароль рута
    # --mount - монтируем каталог mysql внутрь контейнера с валидным путём

    docker run --detach --network <ext_network> --name mariadb --env MYSQL_USER=mariadb_user --env MYSQL_PASSWORD=<password> --env MYSQL_ROOT_PASSWORD=<root_password> --mount "type=bind,src=$(pwd)/mysql,dst=/var/lib/mysql" mariadb:latest

    # или читабельнее
    # ` - символ конца каретки (escape character pwsh)

    docker run `
    --detach `
    --network <ext_network> `
    --name mariadb `
    --env MYSQL_USER=mariadb_user `
    --env MYSQL_PASSWORD=<password> `
    --env MYSQL_ROOT_PASSWORD=<root_password> `
    --mount "type=bind,src=$(pwd)/mysql,dst=/var/lib/mysql" `
    mariadb:latest
<br>

### Запуск Adminer (GUI для БД MariaDB) в своей подсети (CLI)

    # -p - проброс порта наружу (локально:внутри)
    # -e (--env) ADMINER_DEFAULT_SERVER - "подцепляемся" к БД с заданным именем хоста (имя контейнера с базой)

    docker run --detach --network <ext_network> --name adminer -p 8080:8080 -e ADMINER_DEFAULT_SERVER=mariadb adminer:latest

    # или читабельнее

    docker run `
    --detach `
    --network <ext_network> `
    --name adminer `
    -p 8080:8080 `
    -e ADMINER_DEFAULT_SERVER=mariadb `
    adminer:latest
<br>

### Вывести все Container ID списком:
    
    docker ps -a --format '{{.ID}}'
<br>

### Остановить все Container ID по списку:

    docker container stop $(docker ps -a --format '{{.ID}}')
<br>

### Удалить неиспользуемые контейнеры (без подтвеждения):

    docker container prune -f
<br>

### Запуск docker compose (YAML):

    # --file - относительный путь к файлу (можно не указывать при наличии файла {docker-compose|compose}.{y?ml} в . )
    
    docker compose --file .\docker_compose.yaml up --detach
<br>

### Выключение docker compose (YAML):

    ## если compose единственный, то имя можно не указывать

    docker compose down <top-level-аттрибут name: в {docker-compose|compose}.{y?ml}>
<br>

### Смена пароля в MariaDB:

    # входим в контейнер mariadb
    docker exec -it <container ID> bash

    # при входе с другого хоста (если уже настроены и доступны поключения для входа кроме localhost)
    mariadb -u root -h <ip_address> -p

    # выбираем базу
    USE mysql;

    # проверяем под кем зашли
    SELECT CURRENT_USER();

    # смотрим, какие есть пользователи и хосты
    SELECT user,host FROM user;

    # стираем привилегии применные БД в процессе авторизации
    FLUSH PRIVILEGES;

    # меняем пароль пользователя в базе
    ALTER USER 'mariadb_user'@'%' IDENTIFIED BY '<new_user_password>';

    # меняем пароль рута в базе (руту соответствует 2 хоста)
    ALTER USER 'root'@'localhost' IDENTIFIED BY '<new_root_password>';
    ALTER USER 'root'@'%' IDENTIFIED BY '<new_root_password>';

    # выход из базы
    exit; 
