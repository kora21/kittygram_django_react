# Kittygram - блог для размещение фотографий котиков. 
 
### Описание проекта: 
 
Проект Kittygram даёт возможность пользователям поделиться фотографиями своих любимымих котиков. Зарегистрированные пользователи могут создавать, просматривать, редактировать и удалять свои записи. 
 
 
### Установка проекта: 
 
 - Клонироуйте репозиторий:
 
    ```bash
    git clone git@github.com:nadin-belova/kittygram_final.git
    ```
    ```bash
    cd kittygram
    ```
 - Создайте файл .env и заполните его своими данными:

    ```bash
   # Секреты DB
    POSTGRES_USER=[имя_пользователя_базы]
    POSTGRES_PASSWORD=[пароль_к_базе]
    POSTGRES_DB= [имя_базы_данных]
    DB_PORT=[порт_соединения_к_базе]
    DB_HOST=[db]
    ``` 

### Создание Docker-образов

1.  Замените username на ваш логин на DockerHub:

    ```bash
    cd frontend
    docker build -t username/kittygram_frontend .
    cd ../backend
    docker build -t username/kittygram_backend .
    cd ../nginx
    docker build -t username/kittygram_gateway . 
    ```

2. Загрузите образы на DockerHub:

    ```bash
    docker push username/kittygram_frontend
    docker push username/kittygram_backend
    docker push username/kittygram_gateway
    ```
  
### Деплой на удалённый сервере

1. Подключитесь к удаленному серверу

    ```bash
    ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
    ```

2. Создайте на сервере директорию kittygram через терминал

    ```bash
    mkdir kittygram
    ```

3. Установка docker compose на сервер:

    ```bash
    sudo apt update
    sudo apt install curl
    curl -fSL https://get.docker.com -o get-docker.sh
    sudo sh ./get-docker.sh
    sudo apt-get install docker-compose-plugin
    ```

4. В директорию kittygram/ скопируйте файлы docker-compose.production.yml и .env:

    ```bash
    scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
    ```

5. Запустите docker compose в режиме демона:

    ```bash
    sudo docker compose -f docker-compose.production.yml up -d
    ```

6. Выполните миграции, соберите статику бэкенда и скопируйте их в /backend_static/static/:

    ```bash
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
    ```

7. На сервере в редакторе nano откройте конфиг Nginx:

    ```bash
    sudo nano /etc/nginx/sites-enabled/default
   
    ```

8. Добавте настройки location в секции server:

    ```bash
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
    ```

9. Проверьте работоспособность конфигураций и перезапустите Nginx:

    ```bash
    sudo nginx -t 
    sudo service nginx reload
    ```
 
 
### Технологии и необходимые ниструменты: 
- Docker
- Postgres
- Python 3.x 
- Node.js 9.x.x 
- Git 
- Nginx 
- Gunicorn 
- Django (backend) 
- React (frontend) 
 
 
Автор: 
[Ekaterina Tarasenko](https://github.com/kora21/)
