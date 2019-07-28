# morgulnet_platform
ДЗ-4 Kubernetes Volumes
Развернули S3 хранилище в кубернетес.
Создали сервис.
Создали секреты.
Проверил работу через web ui, использовал KubeForwarder.
 Создал бакет ,залил файл и скачал.

ДЗ-2 Kubernetes Security

Task1
01-sa-bob.yaml
 Создаем Service Account bob
02-ClusterRoleBinding-bob.yaml
 Даем бобу роль admin в рамках всего кластера
03-sa-dave.yaml
Создем Service Account dave без доступа к кластеру

Task02
01-namespace-prometheus.yaml
 Создать Namespace prometheus
02-sa-carol.yaml
 Создать Service Account carol Namespace prometheus
03-ClusterRole-Promeththeus.yaml
 Создаем роль возможность делать get, list, watch в отношении Pods всего кластера
04-ClusterRoleBinding-SaInPromeththeus.yaml
 Привязать всем Service Account в Namespace prometheus роль get-list-watch-prometheus


Task03
01-namespace-dev.yaml
 Создать Namespace dev
cat 02-sa-Jane-ns-dev.yaml
 Создать Service Account jane в Namespace dev
cat 03-RoleBinding-Jane-ns-dev.yaml
 Дать jane роль admin в рамках Namespace dev
04-sa-Ken-ns-dev.yaml
 Создать Service Account ken в Namespace dev
05-RoleBinding-Ken-ns-dev.yaml
 Дать ken роль view в рамках Namespace dev

ДЗ-1 Введение в Kubernetes

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
```
http {
    client_body_temp_path /tmp/client_temp;
    proxy_temp_path       /tmp/proxy_temp_path;
    fastcgi_temp_path     /tmp/fastcgi_temp;
    uwsgi_temp_path       /tmp/uwsgi_temp;
    scgi_temp_path        /tmp/scgi_temp;
...
} 
```

Запушим в докерхаб
docker build -t m0rgulnet/web:1.0 .
docker push m0rgulnet/web:1.0

Создал манифест манифест web-pod.yaml

Проверил работу 
kubectl port-forward pods/web 8000:8000
