# CI и CD проекта api_yamdb
Сервис, полностью построенный на API позволяет добавлять в каталог различные произведения, оставлять на них отзывы и видеть динамически собранную оценку. Настроены права доступа для различных категорий пользователей.
### Описание
В данном проекте описаны программы для автоматизированного развёртывания сервиса API на удаленном сервере Ubuntu при помощи GitHub Actions. Добавлены команды последовательно запускающие после пушинга: проверку на стандартизацию pep8 и тестирование кода; загрузку на DockerHub, деплой контейнера с DockerHub на боевой сервер и отправку оповещения в telegram об успешном запуске.
### Технологии
![bop](https://github.com/Polina1Kostina/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
- [Python 3.7]
- [Django 3.2.15]
- [SimpleJWT 4.7.2]
- [Django REST Framework 3.12.4]
- [DRF multiple serializer 0.2.3]
- [Django-filters]
- [Docker-compose]
- [Nginx 1.21.3]
- [PostgreSQL 2.8.6]
- [Gunicorn 20.0.4]
- [GitHub Actions]

# Запуск проекта на удаленном сервере Ubuntu при помощи GitHub Actions:

Клонируйте репозиторий:
```
git clone git@github.com:Polina1Kostina/yamdb_final.git
```
Зайдите через консоль на свой удаленный сервер и установите дополнения docker:
```
ssh <username>@<host>
```
```
sudo systemctl stop nginx # если на сервере ранее были другие сервисы
```
```
sudo apt install docker.io 
```
Установите docker compose:
```
sudo apt -y install curl
```
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```
sudo chmod +x /usr/local/bin/docker-compose
```
Сохраните ssh-ключ для подключения к боевому серверу:
```
cat ~/.ssh/id_rsa
```
Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер:
```
scp docker-compose.yaml <username>@<host>:/home/<username>/docker-compose.yaml
```
```
scp -r nginx/ <username>@<host>:/home/<username>/
```
Создайте на GitHub в разделе settings переменные Secrets:
```
DOCKER_USERNAME # ваш username dockerhub
DOCKER_PASSWORD # ваш пароль dockerhub

HOST # IP-адрес вашего сервера
USER # имя пользователя для подключения к серверу
SSH_KEY
PASSPHRASE # если при создании ssh-ключа вы использовали фразу-пароль

DB_ENGINE
DB_NAME # имя базы данных
POSTGRES_USER # логин для подключения к базе данных
POSTGRES_PASSWORD # пароль для подключения к БД
DB_HOST # название сервиса (контейнера)
DB_PORT # порт для подключения к БД

TELEGRAM_TO # ID вашего телеграм-аккаунта
TELEGRAM_TOKEN # токен вашего бота
```
После успешного деплоя создайте и выполните миграции:
```
sudo docker-compose exec web python manage.py makemigrations
```
```
sudo docker-compose exec web python manage.py migrate
```
Создайте суперпользователя:
```
sudo docker-compose exec web python manage.py createsuperuser
```
Соберите статические файлы:
```
sudo docker-compose exec web python manage.py collectstatic --no-input
```
Если вы хотите наполнить базу данными из фикстур:
```
sudo docker-compose exec web python manage.py loaddata fixtures.json
```
Запустить контейнеры:
```
sudo docker-compose start
```
Для остановки контейнеров:
```
sudo docker-compose stop
```
___
### Примеры GET запросов
Для получения списка доступных адресов отправьте запрос:
```
http://host/api/v1/
```
Пример ответа на запрос о получении списка доступных адресов в формате json:
```
{
    "users": "http://host/api/v1/users/",
    "titles": "http://host/api/v1/titles/",
    "categories": "http://host/api/v1/categories/",
    "genres": "http://host/api/v1/genres/"
}
```
Для получения всех произведений:
```
GET http://host/api/v1/titles/
```
Пример ответа на запрос о получении всех произведений в формате json:
```
[
    {
        "count": 1,
        "next": "Null",
        "previous": "Null",
        "results": [
            {
                "id": 1,
                "name": "The best team",
                "year": 2022,
                "rating": 10,
                "description": "Little story about dev team",
                "genre": [
                    {
                        "name": "Adventure",
                        "slug": "adventure"
                    }
                ],
                "category": {
                "name": "IT",
                "slug": "it"
                }
            }
        ]
    }
]
```
Все виды запросов и их описание доступно в документации по адресу:
```
http://host/redoc/
```
### Автор дополнения для автоматизированного развёртывания на удаленном сервере Ubuntu при помощи GitHub Actions
- [Полина Костина], студентка Яндекс Практикума
#### [Мой проект на удаленном сервере](http://158.160.33.167/redoc/>)

### Авторы сервиса API
- [Никита Емельянов], студент Яндекс Практикума он же тимлид на этом проекте.
- [Полина Костина], студентка Яндекс Практикума
- [Дмитрий Ротанин], студент Яндекс Практикума


[//]: # (Ниже находятся справочные ссылки)

   [Python 3.7]: <https://www.python.org/downloads/release/python-370/>
   [Django 3.2.15]: <https://www.djangoproject.com/download/>
   [SimpleJWT 4.7.2]: <https://django-rest-framework-simplejwt.readthedocs.io/en/latest/>
   [Django REST Framework 3.12.4]: <https://www.django-rest-framework.org/community/release-notes/>
   [DRF multiple serializer 0.2.3]: <https://pypi.org/project/drf-multiple-serializer/>
   [Django-filters]: <https://django-filter.readthedocs.io/en/stable/guide/install.html>
   [Docker-compose]: <https://docs.docker.com/compose/gettingstarted/>
   [Nginx 1.21.3]: <https://nginx.org/ru/docs/beginners_guide.html>
   [PostgreSQL 2.8.6]: <https://www.postgresql.org/docs/>
   [Gunicorn 20.0.4]: <https://docs.gunicorn.org/en/stable/install.html>
   [GitHub Actions]: <https://docs.github.com/en/actions>
   [Никита Емельянов]: <https://github.com/Tozix>
   [Полина Костина]: <https://github.com/Polina1Kostina>
   [Дмитрий Ротанин]: <https://github.com/Annsjaw>
