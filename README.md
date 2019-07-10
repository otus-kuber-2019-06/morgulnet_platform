# morgulnet_platform
ДЗ-1
Создать Dockerfile, в котором будет описан образ:
1. Запускающий web-сервер на порту 8000 (можно использовать
любой способ)
2. Отдающий содержимое директории /app внутри контейнера
(например, если в директории /app лежит файл homework.html,
то при запуске контейнера данный файл должен быть доступен
по URL http://localhost:8000/homework.html)
3. Работающий с UID 1001

Подглядел у коллег все использовали  python -m http.server
Немного подумав решил использовать стандарт дефакто nginx
По умолчанию стандартный образ nginx работает на 80 порту.
Поэтому переписываю дефолтный конфиг и добавляю локейшен на app.
C изменением uid помогла инструкция 
Running nginx as a non-root user
https://docs.docker.com/samples/library/nginx/


Как работать со стандартным образом nginx

Dockerfile
FROM nginx
COPY static-html-directory /usr/share/nginx/html
docker build -t some-content-nginx .
docker run --name web1.0 -d -p 8080:80 m0rgulnet/web:1.0
curl http://localhost:8080

Что сделать чтобы работало не под рутом

pid        /tmp/nginx.pid;
http {
    client_body_temp_path /tmp/client_temp;
    proxy_temp_path       /tmp/proxy_temp_path;
    fastcgi_temp_path     /tmp/fastcgi_temp;
    uwsgi_temp_path       /tmp/uwsgi_temp;
    scgi_temp_path        /tmp/scgi_temp;
...
}

Запушим в докерхаб
docker build -t m0rgulnet/web:1.0 .
docker push m0rgulnet/web:1.0



