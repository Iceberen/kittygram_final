# Проект «Kittygram»

## Описание проекта:
«Kittygram» соцсеть для обмена фотографиями и достижениями своих питомцев.  
Зарегистрированные пользователи соцсети могут постить фотографии питомцев,  
добавлять постам описание, а также смотреть на посты других пользователей.
#### Развернутый проект:
[https://infra-sprint1.zapto.org](https://infra-sprint1.zapto.org)
#### Cостояние рабочего процесса в GitHub:
![CI Status](https://github.com/Iceberen/kittygram_final/actions/workflows/main.yml/badge.svg)

# Как запустить проект:
*Примечание: Все примеры указаны для Linux*
## Локально
*Примечание: Для локального запуска проекта используется `docker-compose.yml`*
### Установка
- Клонировать репозиторий и перейти в него в командной строке:
  ```
  git clone https://github.com/Iceberen/kittygram_final.git
  cd kittygram_final
  ```
- Создать файл `.env` и заполните его своими данными. Все необходимые переменные перечислены в файле `.env.example.`
### Запуск `Docker compose`
1. Из корневой папки проекта выполните команду:
  ```
  docker compose up 
  ```
2. Из корневой папки проекта выполните миграции:
  ```
  docker compose exec backend python manage.py migrate 
  ```
3. Из корневой папки проекта выполните команды сборки и копирования статики:
  ```
  docker compose exec backend python manage.py collectstatic
  docker compose exec backend cp -r /app/collected_static/. /static/static/
  ```

## На удаленном сервере
*Примечание: Для запуска проекта на удаленном сервере используется `docker-compose.production.yml`*
### Установка
- Клонировать репозиторий и перейти в него в командной строке:
  ```
  git clone https://github.com/Iceberen/kittygram_final.git
  cd kittygram_final
  ```
- Создать файл `.env` и заполните его своими данными. Все необходимые переменные перечислены в файле `.env.example.`

### Создание Docker-образов
1. Из корневой папки проекта выполните команды из листинга при этом замените `username` на свой логин на `DockerHub`:
  ```
  cd frontend
  docker build -t username/kittygram_frontend .
  cd ../backend
  docker build -t username/kittygram_backend .
  cd ../nginx
  docker build -t username/kittygram_gateway . 
  ```
2. Загрузите образы на `DockerHub`, заменив `username` на свой логин на `DockerHub`:
  ```
  docker push username/kittygram_frontend
  docker push username/kittygram_backend
  docker push username/kittygram_gateway
  ```

### Деплой на сервере
1. Подключитесь к удаленному серверу:
  ```
  ssh -i "путь_до_ключа_SSH"/"имя_файла_ключа_SSH" username@IP_adress_server
  ```
2. Создайте на сервере директорию `kittygram`:
  ```
  mkdir kittygram
  ```
3. Установите `Docker Compose` на сервер:
  ```
  sudo apt update
  sudo apt install curl
  curl -fsSL https://get.docker.com -o get-docker.sh
  sudo sh get-docker.sh
  sudo apt install docker-compose
  ```
4. Скопируйте файлы `docker-compose.production.yml` и `.env` в директорию `kittygram/` на сервере:
  ```
  scp -i "путь_до_ключа_SSH"/"имя_файла_ключа_SSH" docker-compose.production.yml /  
  username@IP_adress_server:/home/username/kittygram/docker-compose.production.yml
  ```
  и
  ```
  scp -i "путь_до_ключа_SSH"/"имя_файла_ключа_SSH" .env /  
  username@IP_adress_server:/home/username/kittygram/.env
  ```
5. Запустите `Docker Compose` в режиме демона:
  ```
  sudo docker-compose -f /home/username/kittygram/docker-compose.production.yml up -d
  ```
6. Выполните миграции, соберите статические файлы бэкенда и скопируйте их в `/static/static/`:
  ```
  sudo docker-compose -f /home/username/kittygram/docker-compose.production.yml exec backend python manage.py migrate
  sudo docker-compose -f /home/username/kittygram/docker-compose.production.yml exec backend python manage.py collectstatic
  sudo docker-compose -f /home/username/kittygram/docker-compose.production.yml exec backend cp -r /app/collected_static/. /static/static/
  ```
7. Откройте конфигурационный файл `Nginx` в редакторе `nano`:
  ```
  sudo nano /etc/nginx/sites-enabled/default
  ```
8. Измените настройки `location` в секции `server`:
  ```
  location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
  }
  ```
9. Проверьте правильность конфигурации `Nginx`:
  ```
  sudo nginx -t
  ```
10. Перезапустите `Nginx`:
  ```
  sudo service nginx reload
  ```

## Настройка CI/CD
1. Файл `workflow` уже написан и находится в директории:
  ```
  /.github/workflows/main.yml
  ```
2. Добавьте секреты в GitHub Actions:
  ```
  DOCKER_USERNAME                # имя пользователя в DockerHub
  DOCKER_PASSWORD                # пароль пользователя в DockerHub
  HOST                           # IP-адрес сервера
  USER                           # имя пользователя
  SSH_KEY                        # содержимое приватного SSH-ключа
  SSH_PASSPHRASE                 # пароль для SSH-ключа
  TELEGRAM_TO                    # ID вашего телеграм-аккаунта
  TELEGRAM_TOKEN                 # токен вашего бота
  ```

## Технологии
- Python
- Django
- DjangoRestFramework
- PostgreSQL

## Автор
[Васильев Вячеслав](https://github.com/Iceberen)