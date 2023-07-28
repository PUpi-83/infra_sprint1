# Kittygram - минималистичная социальная сеть для любителей котиков! :cat2:

## О проекте
**Kittygram** - это проект, разработанный в рамках учебного курса по Python от Яндекс.Практикум. Это уникальная социальная платформа, предназначенная исключительно для обмена фотографиями котиков! Здесь вы можете зарегистрироваться, загружать снимки своих милых котиков, добавлять к ним краткие описания и наслаждаться просмотром котиков других пользователей.
***Если хочется посмотреть как заранее выглядит сайт после деплоя, то переходите [сюда](https://purrterritory.sytes.net/)***
## Основные возможности
-   Простая регистрация и логин с помощью учетной записи пользователя.
-   Загрузка фотографий котиков с добавлением к ним кратких описаний.
-   Просмотр фотографий и описаний котиков других пользователей.

## Технологии
-   Python 3.9
-   Django==3.2.3
-   djangorestframework==3.12.4
-   Nginx
-   Gunicorn


## Установка проекта на локальный компьютер из репозитория

- **Клонирование репозитория**
```git clone <адрес вашего репозитория>```
   *Замените <адрес вашего репозитория> на URL вашего репозитория на GitHub или другой хостинг-сервис.*
    
- **Переход в директорию с клонированным репозиторием**
    `cd kittygram` 
    
-  **Создание виртуального окружения**
    `python3 -m venv venv` 
    *Это поможет изолировать зависимости проекта от других проектов на вашем компьютере.*
 
-  **Установка зависимостей**
    `pip install -r requirements.txt` 
    
    ***Убедитесь, что у вас установлен Python и pip, чтобы успешно выполнить эту  команду.***
    
-  **Создание файла .env** В директории `/backend/kittygram_backend/` создайте файл с именем `.env`.
    
-  **Прописать SECRET_KEY в файле .env** В файле `.env` добавьте следующую строку и замените `<ваш_ключ>` на секретный ключ вашего приложения:
    `SECRET_KEY = '<ваш_ключ>'` 

# Деплой проекта на удаленный сервер

## Подключение сервера к аккаунту на GitHub

-  **Для того, чтобы успешно подключить ваш сервер к аккаунту на GitHub, убедитесь, что на сервере установлен Git. Вы можете проверить его наличие и версию, выполнив следующую команду:**
      `sudo apt update`
      `git --version` 

-  **Если Git не установлен, выполните установку с помощью команды:**
      `sudo apt install git` 

-  **Затем, находясь на вашем сервере, сгенерируйте SSH-ключи с помощью следующей команды:**
     `ssh-keygen` 

-  **Сохраните открытый ключ (public key) в вашем аккаунте на GitHub. Для этого выведите содержимое ключа в терминал с помощью команды:**
    `cat ~/.ssh/id_rsa.pub` 

-  **Скопируйте ключ, начиная от символов "ssh-rsa" и включительно, и заканчивая концом ключа. После этого добавьте скопированный ключ к вашему аккаунту на GitHub.**

-  **Теперь вы можете клонировать проект с GitHub на ваш сервер, используя команду:**
    `git clone git@github.com:Ваш_аккаунт/<Имя проекта>.git`

## Запуск backend проекта на сервере

*Чтобы успешно запустить backend проекта на вашем сервере, выполните следующие шаги:*
-  **Установка пакетного менеджера и утилиты для создания виртуального окружения:**
    `sudo apt install python3-pip python3-venv -y` 
    
-  **Создание и активация виртуального окружения в директории с проектом:**
    ```python3 -m venv venv```
    ```source venv/bin/activate``` 
    
-  **Установка необходимых зависимостей проекта:**
    ```pip install -r requirements.txt``` 
    
-  **Выполнение миграций базы данных:**
    ```python manage.py migrate``` 
    
-  **Создание суперпользователя для администрирования проекта:**
    ```python manage.py createsuperuser``` 
    
-  **Редактирование файла `settings.py` на сервере.
Откройте файл `settings.py`, который находится в папке проекта, и найдите переменную `ALLOWED_HOSTS`. Добавьте в нее внешний IP-адрес вашего сервера, а также адреса `127.0.0.1` и `localhost`, чтобы сервер мог обрабатывать запросы от этих адресов.** 
    ```ALLOWED_HOSTS = ['<внешний адрес вашего сервера>', '127.0.0.1', 'localhost']```

## Запуск frontend проекта на сервере

***Для запуска frontend проекта на вашем сервере выполните следующие шаги:***

- **Установка Node.js на сервер:**
    ```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - && sudo apt-get install -y nodejs``` 
    
- **Установка зависимостей frontend приложения: Перейдите в директорию `<ваш проект>/frontend/` и выполните команду:**
    ```npm i```

## Установка и запуск Gunicorn
-  **Активируйте виртуальное окружение своего проекта.**
    
-  **Установите пакет Gunicorn версии 20.1.0 с помощью команды:**
    ```pip install gunicorn==20.1.0``` 
    
-  **Откройте файл settings.py вашего проекта и установите значение `DEBUG` на `False`:**
    ```DEBUG = False``` 
    
-  **В директории `/etc/systemd/system/` создайте файл `gunicorn.service` с помощью текстового редактора, например, используя `nano`:**
    `sudo nano /etc/systemd/system/gunicorn.service` 
    
-  **Вставьте следующий код в файл `gunicorn.service` (без комментариев):**
    ```
    [Unit]
    Description=gunicorn daemon
    After=network.target
    
    [Service]
    User=yc-user
    WorkingDirectory=/home/<имя пользователя в системе>/<имя проекта>/backend/
    ExecStart=/home/<имя пользователя в системе>/<имя проекта>/venv/bin/gunicorn --bind 0.0.0.0:8080 kittygram_backend.wsgi
    
    [Install]
    WantedBy=multi-user.target
    ```
     *Замените `<имя пользователя в системе>` и `<имя проекта>` на соответствующие значения.*
    
- **Чтобы узнать точный путь до Gunicorn при активированном виртуальном окружении, выполните команду:**
    `which gunicorn` 

- **Теперь Gunicorn установлен и настроен как служба systemd, и вы можете запустить его с помощью команды:**
  `sudo systemctl start gunicorn` 
  
- **Также можно добавить службу Gunicorn в автозагрузку при запуске системы:**
  `sudo systemctl enable gunicorn`

## Установка и настройка Nginx
- **Установка Nginx на сервере. Откройте терминал и выполните следующую команду, чтобы установить Nginx:**
`sudo apt install nginx -y`

- **Ограничение доступа к портам. Чтобы установить ограничения на открытые порты, выполните поочередно две команды:**
`sudo ufw allow 'Nginx Full'`
`sudo ufw allow OpenSSH`

- **Включение файервола. Активируйте файервол с помощью следующей команды:**
`sudo ufw enable`

- **Сборка и размещение статики frontend-приложения. Перейдите в директорию <имя_проекта>/frontend/ и выполните команду для сборки статики:**
`npm run build`

  **Результат сборки будет сохранен в директории .../frontend/build/. Теперь скопируйте содержимое папки /frontend/build/ в системную директорию сервера /var/www/.**
  `sudo cp -r путь_к_директории_с_фронтенд-приложением/build/. /var/www/имя_проекта/`

- **Настройка файла конфигурации Nginx. Откройте файл конфигурации веб-сервера в текстовом редакторе:**
`sudo nano /etc/nginx/sites-enabled/default`

  **Замените его содержимое следующим кодом:**
  ```
  server {
    listen 80;
    server_name публичный_ip_вашего_удаленного_сервера;

    location / {
        root   /var/www/<имя_проекта>;
        index  index.html index.htm;
        try_files $uri /index.html;
    }
    
    location /media/ {
        alias /var/www/kittygram/media/;
    }
    
    location /api/ {
        proxy_pass http://127.0.0.1:8080;
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:8000;
    }
  } 
- **Проверка корректности конфигурации. Убедитесь, что конфигурация Nginx корректна, выполнив команду:**
`sudo nginx -t`

- **Перезагрузка конфигурации Nginx. Если проверка прошла успешно, перезагрузите конфигурацию Nginx:**
`sudo systemctl reload nginx`

- **Сборка и настройка статики для backend-приложения. В файле settings.py вашего backend-приложения пропишите следующие настройки:**
`STATIC_URL = "/static_backend/"`
`STATIC_ROOT = BASE_DIR / "static_backend"`

- **Затем активируйте виртуальное окружение вашего проекта, перейдите в директорию с файлом manage.py и выполните команду:**
`python manage.py collectstatic`
*После выполнения команды будет создана директория static_backend/ в директории_<имя_проекта>/backend/_.*

- **Копирование статики backend-приложения. Скопируйте директорию static_backend/ в директорию /var/www/<имя_проекта>/:**
`sudo cp -r путь_к_директории_с_бэкендом/static_backend /var/www/название_проекта`

## Добавление доменного имени в настройки Django
- **Откройте файл `settings.py`, который находится в вашем проекте, и найдите переменную `ALLOWED_HOSTS`.**

- **Внесите необходимые изменения, добавив ваше доменное имя в список `ALLOWED_HOSTS`. Результат должен выглядеть примерно так:**
`ALLOWED_HOSTS = ["ip_адрес_вашего_сервера", "127.0.0.1", "localhost", "ваш-домен"]`

- **Сохраните изменения в файле `settings.py`**

- **Теперь перезапустите сервер Gunicorn, чтобы изменения вступили в силу. Выполните команду:**
`sudo systemctl restart gunicorn`

- **Далее внесите необходимые изменения в конфигурацию Nginx. Откройте конфигурационный файл с помощью текстового редактора (например, nano) и выполните команду:**
`sudo nano /etc/nginx/sites-enabled/default`

- **В открывшемся файле найдите блок `server` и в строку `server_name` добавьте ваше доменное имя (или IP-адрес) через пробел. Например:**
`server {
...
    server_name ваш-домен или-IP-адрес;
...
}`
*Пожалуйста, убедитесь, что вместо "ваш-домен" и "или-IP-адрес" вы замените на фактические значения вашего доменного имени и/или IP-адреса.*

- **Проверьте корректность конфигурации Nginx с помощью команды:**
`sudo nginx -t`
**Если конфигурация верна, перезагрузите Nginx, чтобы изменения вступили в силу:**
`sudo systemctl reload nginx`

## Получение и настройка SSL-сертификата
### Шаг 1: Установите необходимые инструменты
#### Откройте терминал на сервере и выполните следующие команды поочередно:

- **Установите пакет `snapd`, если его еще нет:**
`sudo apt install snapd`
 
 - **Установите и обновите `core`:**
`sudo snap install core; sudo snap refresh core`

- **Установите `certbot` с помощью Snap:**
`sudo snap install --classic certbot`

- **Создайте символическую ссылку на `certbot`, чтобы использовать его из любой директории:**
`sudo ln -s /snap/bin/certbot /usr/bin/certbot`

### Шаг 2: Получите SSL-сертификат 
- **Теперь, когда необходимые инструменты установлены, выполните следующую команду, чтобы запустить `certbot` и получить SSL-сертификат:** 
`sudo certbot --nginx`
*SSL-сертификат будет автоматически сохранен на вашем сервере в системной директории `/etc/ssl/`. Также `certbot` автоматически изменит конфигурацию Nginx, добавив новые настройки в файл `/etc/nginx/sites-enabled/default` и прописав пути к сертификату.*
### Шаг 3: Перезагрузите Nginx

- **Наконец, чтобы применить изменения, перезагрузите конфигурацию Nginx с помощью следующей команды:**
`sudo systemctl reload nginx`

***На этом шаге развертывание проекта закончено, если вы следовали всем шагам, то уверена, что у вас всё получилось!***

## [Автор: Анастасия](https://github.com/PUpi-83)
