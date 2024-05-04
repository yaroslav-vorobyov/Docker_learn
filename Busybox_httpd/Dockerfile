FROM busybox:latest
# создаём рабочий каталог для httpd
RUN mkdir /var/www/html
# переопределяем рабочий каталог и тип запуска
CMD [ "httpd", "-f", "-h", "/var/www/html"]
