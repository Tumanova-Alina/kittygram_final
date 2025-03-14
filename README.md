# Kittygram - социальная сеть для размещения фотографий котиков. 
 
### Описание проекта: 
 
Kittygram - проект для любителей котиков и их владельцев. Зарегистрированные пользователи могут делиться фотографиями своих питомцев и их достижениями, удалять и редактировать посты. 

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
 
 - Клонирование репозитория:
 
    ```bash
    https://github.com/Tumanova-Alina/kittygram_final.git
    ```
    ```bash
    cd kittygram
    ```

 - Создание файла .env:

    ```bash
   POSTGRES_USER=логин_от_бд
   POSTGRES_PASSWORD=пароль_от_бд
   POSTGRES_DB=название_бд
   DB_HOST=название_хоста
   DB_PORT=5432
   SECRET_KEY=django_settings_secret_key
   ALLOWED_HOSTS=127.0.0.1, localhost
    ```

 - Создание repository secrets в GitHub Actions:

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

 - Замена username на ваш логин на DockerHub:

    ```bash
    cd frontend
    docker build -t username/kittygram_frontend .
    cd ../backend
    docker build -t username/kittygram_backend .
    cd ../nginx
    docker build -t username/kittygram_gateway . 
    ```

 - Загрузка образов на DockerHub:

    ```bash
    docker push username/kittygram_frontend
    docker push username/kittygram_backend
    docker push username/kittygram_gateway
    ```
  
### Деплой на удалённый сервер:

 - Подключение к удаленному серверу:

    ```bash
    ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
    ```

 - Создание на сервере директории kittygram через терминал:

    ```bash
    mkdir kittygram
    ```

 - Установка docker compose на сервер:

    ```bash
    sudo apt update
    sudo apt install curl
    curl -fSL https://get.docker.com -o get-docker.sh
    sudo sh ./get-docker.sh
    sudo apt-get install docker-compose-plugin
    ```

 - Копирование файлов docker-compose.production.yml и .env в директорию kittygram/ :

    ```bash
    scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
    ```

 - Запуск docker compose в режиме демона:

    ```bash
    sudo docker compose -f docker-compose.production.yml up -d
    ```

 - Выполнение миграций, сбор статики бэкенда и копирование их в /backend_static/static/:

    ```bash
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
    ```

 - На сервере открытие в редакторе nano конфига nginx:

    ```bash
    sudo nano /etc/nginx/sites-enabled/default
   
    ```

 - Добавление настройки location в секции server:

    ```bash
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
    ```

 - Проверка работоспособности конфигураций и перезапуск Nginx:

    ```bash
    sudo nginx -t 
    sudo service nginx reload
    ```
 
 
###  Cсылка на приложение kittygram: 
 
- #### https://kittycatsgram.work.gd/


## Автор:
+ **Алина Туманова** [Tumanova-Alina](https://github.com/Tumanova-Alina)