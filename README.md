# yamdb_final
Проект разработан в рамках обучения на курсе Backend-разработчик Яндекс Практикума и содержит в себе все необходимые инструкции и файлы для автоматизированного развёртывания сервиса API на удаленном сервере Ubuntu при помощи GitHub Actions.
### Описание
Сервис, полностью построенный на API позволяет добавлять в каталог различные произведения, оставлять на них отзывы и видеть динамически собранную оценку. Настроены права доступа для различных категорий пользователей.
В данном проекте описаны программы для самостоятельного развёртывания на HTTP-сервере nginx с помощью утилиты Docker-compose. Также добавлены команды GitHub Actions, последовательно запускающие после пушинга: проверку на стандартизацию pep8 и тестирование кода; загрузку на DockerHub, деплой контейнера с DockerHub на боевой сервер и отправку оповещения в telegram об успешном запуске.
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
docker-compose exec web python manage.py makemigrations
```
```
docker-compose exec web python manage.py migrate
```
Создайте суперпользователя:
```
docker-compose exec web python manage.py createsuperuser
```
Соберите статические файлы:
```
docker-compose exec web python manage.py collectstatic --no-input
```
Если вы хотите наполнить базу данными из фикстур:
```
docker-compose exec web python manage.py loaddata fixtures.json
```
Для остановки контейнеров:
```
### Автор дополнения для автоматизированного развёртывания на удаленном сервере Ubuntu при помощи GitHub Actions
- [Полина Костина], студентка Яндекс Практикума

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