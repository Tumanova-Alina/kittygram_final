# Kittygram - социальная сеть для размещения фотографий котиков. 
 
### Описание проекта: 
 
Kittygram - проект, который даёт возможность пользователям делиться фотографиями и достижениями своих обожаемых котиков. Зарегистрированные пользователи могут создавать, редактировать, просматривать и удалять свои записи. 

### Технологии: 
- Docker
- PostgreSQL
- Python 3.9
- Node.js
- Git 
- Nginx 
- Gunicorn 
- Django (backend) 
- React (frontend)
- Github Actions
 
### Запуск проекта: 
 
 - Клонируйте репозиторий:
 
    ```bash
    https://github.com/Tumanova-Alina/kittygram_final.git
    ```
    ```bash
    cd kittygram
    ```

 - Создайте файл .env и заполните его своими данными:

    ```bash
   POSTGRES_USER=логин_от_бд
   POSTGRES_PASSWORD=пароль_от_бд
   POSTGRES_DB=название_бд
   DB_HOST=название_хоста
   DB_PORT=5432
   SECRET_KEY=django_settings_secret_key
   ALLOWED_HOSTS=127.0.0.1, localhost
    ```

 - Создайте repository secrets в GitHub Actions:

    ```
    DOCKER_USERNAME=логин в Docker Hub
    DOCKER_PASSWORD=пароль для Docker Hub
    SSH_KEY=закрытый SSH-ключ для доступа к продакшен-серверу
    SSH_PASSPHRASE=passphrase для этого ключа
    USER=username для доступа к продакшен-серверу
    HOST=адрес хоста для доступа к продакшен-серверу
    TELEGRAM_TO=ID своего телеграм-аккаунта
    TELEGRAM_TOKEN=токен telegram-бота
    ``` 

### Создание Docker-образов:

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
  
### Деплой на удалённый сервер:

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

8. Добавьте настройки location в секции server:

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
 
 
###  Cсылка на приложение kittygram: 
 
- #### https://kittycatsgram.work.gd/


## Автор:
+ **Алина Туманова** [Tumanova-Alina](https://github.com/Tumanova-Alina)