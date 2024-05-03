# Тренировочный репозиторий по Docker
## Содержание репозитория:

| Название проекта | Описание |
| :--- | --- |
| [MyApp_cowsay](https://github.com/yaroslav-vorobyov/Docker_learn/tree/main/MyApp_cowsay) | Сборка контейнера c Dockerfile, приложение Cowsay |
| [Docker-compose + network + MariaDB + Adminer](https://github.com/yaroslav-vorobyov/Docker_learn/tree/main/Docker-compose_network) | Сборка Docker compose c yaml, своя подсеть + пароли к базе + volume'ы.<br> Есть возможность запустить два контейнера вручную из CLI (конечный результат идентичен с docker compose, с docker результат более элегантный, пароли вынесены во внешние файлы (лучше, чем "светить" их в CLI, пока без Docker secrets), БД использует внешний volume и предварительно созданную свою подсеть |
| [Docker compose + docker volumes](https://github.com/yaroslav-vorobyov/Docker_learn/tree/main/Ubuntu-demo-volume) | Сборка Docker compose c yaml + volume'ы.<br> Закрепление работы с volume (external: true / false), закрепление *контекста* `build`, *аттрибуты* `tty` & `stdin_open` |
| [Docker compose + docker volumes](https://github.com/yaroslav-vorobyov/Docker_learn/tree/main/Ubuntu_only_ssh_block_cli) | Сборка образа c Dockerfile и доступом по SSH.<br> Закрепление работы с docker ssh, выборка параметров контейнера из json'а, ограничение доступа в *контексте* по ssh |