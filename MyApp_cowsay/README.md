### Собрать образ из Dockerfile:
[Docker Docs - Build](https://docs.docker.com/reference/cli/docker/image/build/#text-files)

    # под pwsh (windows)
    Get-Content .\Dockerfile | docker build - -t <docker_id>/myapp

    # универсальная сборка образа
    docker build -t <docker_id>/myapp .

### Удалить остановленные и неиспользуемые контейнеры без дополнительных вопросов:
    docker container prune -f

### Запустить образ с выводом результата и удалить контейнер
    docker run --rm <docker_id>/myapp -f tux "WoW"

### Справка по <ins>**cowsay**</ins>:
    docker run --rm <docker_id>/myapp -h