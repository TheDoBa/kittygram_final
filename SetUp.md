## Развёртывание проекта:
+ Клонировать репозиторий и перейти в него в командной строке:
```shell script
git clone git@github.com:TheDoBa/kittygram_final.git
```

```shell script
cd kittygram_final/
```

+ Cоздать и активировать виртуальное окружение (Windows/Bash):
```shell script
python -m venv venv
```

```shell script
source venv/Scripts/activate
```

+ Установить зависимости из файла requirements.txt:
```shell script
python -m pip install --upgrade pip
```

```shell script
pip install -r backend/requirements.txt
```

+ Установите [Docker compose](https://www.docker.com/) на свой компьютер.

+ Запустите проект через docker-compose:
```shell script
docker compose -f docker-compose.production.yml up --build -d
```

+ Выполнить миграции:
```shell script
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```

+ Соберите статику:
```shell script
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
```

+ Скопируйте статику:
```shell script
docker compose -f docker-compose.production.yml exec backend cp -r /app/static_backend/. /backend_static/static/
```

+ Создать файл `.env` с переменными окружениями:

[Примеры переменных окружения](./.env.example)

<br>

## Workflows:

Continuous Integration (CI) и Continuous Deployment (CD) реализовано через `GitHub Actions` 

Для работы проекта необходимо прописать переменные окружения в `Settings/Secrets and variables/Actions/New repository secret`:

```txt
# имя пользователя в Docker
NICKNAME

# имя пользователя в DockerHub
DOCKER_USERNAME    

# пароль пользователя в DockerHub
DOCKER_PASSWORD    

# ip_address сервера
SSH_HOST      

# имя пользователя                     
SSH_USER    

# приватный ssh-ключ (cat ~/.ssh/id_rsa)
SSH_KEY    

# пароль для ssh-ключа            
SSH_PASSPHRASE                   

# id телеграм-аккаунта (@userinfobot)
TELEGRAM_TO      

# токен вышего бота (@BotFather)
TELEGRAM_TOKEN                 
```

При `push` в ветку `main` автоматически отрабатывают сценарии:

+ `Tests 3.9, 3.10, 3.11` - проверка кода на соответствие стандарту `PEP8` и запуск `Pytest`;
+ `Push Docker image to DockerHub`, `Push gateway Docker image to DockerHub`, `Push frontend Docker image to DockerHub` - сборка и доставка докер-образов на DockerHub;
+ `Deploy` - автоматический деплой проекта на рабочий сервер;
+ `Send message` - отправка уведомления в Telegram.

<br>

## Тестирование проекта:
Тестирование реализовано с использованием библиотеки Pytest. 

+ Запустить тесты из основной директории проекта:
```shell script
pytest
```