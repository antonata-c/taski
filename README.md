![Main kittygram workflow](https://github.com/antonata-c/taski/actions/workflows/main.yml/badge.svg?event=push)
`Python` `Django` `Django REST Framework` `PostgreSQL` `Nginx` `Gunicorn` `Docker` `Docker Compose` `JS`
##  Taski
### Описание
Taski - Приложение для планирования задач. Задачи можно добавлять, изменять, удалять
и переводить из группы "незавершённые" в "завершённые". Деплой производится на удаленный сервер.
***
### Развертывание проекта
Подключитесь к удалённому серверу
```
ssh -i путь_до_SSH_ключа/название_файла_с_SSH_ключом_без_расширения login@ip
```
Клонируйте проект на сервер и установите Docker Compose
```
git clone git@github.com:antonata-c/taski.git
sudo apt update
sudo apt install curl
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo apt install docker-compose 
cd taski/backend/
```
В директории `gateway`, выполняем следующие команды
```
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic --noinput
```
***
#### Nginx
- Установите и настройте веб- и прокси-сервер Nginx
```shell
sudo apt install nginx -y
sudo systemctl start nginx
sudo nano /etc/nginx/sites-enabled/default 
```
- Запишите новые настройки для блока server проекта taski:
```text
server {
        server_name ваш_домен;

        server_tokens off;

        location / {
            proxy_set_header        Host $host;
            proxy_set_header        X-Real-IP $remote_addr;
            proxy_set_header        X-Forwarded-Proto $scheme;
            proxy_pass http://127.0.0.1:8000;
        }
}
```
- Сохраните изменения, протестируйте и перезагрузите конфигурацию nginx
```shell
sudo nginx -t
sudo systemctl reload nginx
```
#### Проект готов к использованию!
***
### Автор работы:
**[Антон Земцов](https://github.com/antonata-c)**
