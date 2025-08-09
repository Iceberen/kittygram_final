# 🐾 Kittygram — социальная сеть для любителей котиков

**Kittygram** — это веб-приложение, где пользователи могут публиковать фото своих котов, описывать их и делиться достижениями питомцев.
Проект создан на **DRF** и развёрнут с помощью **Docker**.

---

## Описание проекта:
«Kittygram» соцсеть для обмена фотографиями и достижениями своих питомцев.
Зарегистрированные пользователи соцсети могут постить фотографии питомцев,
добавлять постам описание, а также смотреть посты других пользователей.

---

## 🔗 Развёрнутый проект:
[https://infra-sprint1.zapto.org](https://infra-sprint1.zapto.org).  
![CI Status](https://github.com/Iceberen/kittygram_final/actions/workflows/main.yml/badge.svg)

---

## 🚀 Технологии
- Python 3.11
- Django
- Django REST Framework
- PostgreSQL
- Docker & Docker Compose
- Nginx
- CI/CD

---

## ⚙️ Локальный запуск
1. Клонировать репозиторий и перейти в него в командной строке:
  ```
  git clone https://github.com/Iceberen/kittygram_final.git
  cd kittygram_final
  ```
2. Создать и заполнить файл .env. Все необходимые переменные в файле `.env.example`
  ```
  cp .env.example .env
  # Откройте .env и укажите свои настройки
  ```
3. Запустить проект в Docker
  ```
  docker compose up -d --build
  ```
4. Применить миграции и собрать статику
  ```
  docker compose exec backend python manage.py migrate
  docker compose exec backend python manage.py collectstatic
  docker compose exec backend cp -r /app/collected_static/. /static/static/
  ```
5. Готово!
  Приложение будет доступно по адресу:
  ```
  http://localhost/
  ```

---

## ☁️ Деплой на сервер

1. Собрать образы:
  ```
  cd frontend && docker build -t username/kittygram_frontend .
  cd ../backend && docker build -t username/kittygram_backend .
  cd ../nginx && docker build -t username/kittygram_gateway .
  ```
2. Отправить образы на DockerHub:
  ```
  docker push username/kittygram_frontend
  docker push username/kittygram_backend
  docker push username/kittygram_gateway
  ```
3. Подключиться к серверу и запустить:
  ```
  ssh -i ~/.ssh/key username@server_ip
  mkdir kittygram
  sudo docker-compose -f kittygram/docker-compose.production.yml up -d
  ```

---

## 🤖 CI/CD
1. Процесс автоматизирован через **GitHub Actions**:
- Сборка и публикация образов на DockerHub
- Деплой на удалённый сервер
- Уведомления в Telegram
**Секреты в GitHub Actions:**
  ```
  DOCKER_USERNAME
  DOCKER_PASSWORD
  HOST
  USER
  SSH_KEY
  SSH_PASSPHRASE
  TELEGRAM_TO
  TELEGRAM_TOKEN
  ```

---

## 📂 Структура проекта
```
kittygram_final/
├── backend/         # Django-приложение: модели, API, админка
├── frontend/        # React-приложение для интерфейса
├── nginx/           # Конфигурация веб-сервера
├── tests/           # Автотесты
├── docker-compose.yml
├── docker-compose.production.yml
└── .env.example     # Пример конфигурации окружения
```

---

## 🧑‍💻 Автор

Разработано: [Iceberen](https://github.com/Iceberen) в рамках учебного спринта по управлению проектом на удаленном сервере.

---