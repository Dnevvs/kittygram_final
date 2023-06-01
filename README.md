# Kittygram_final   
## **Деплой проекта Kittygram на удалённый сервер c помощью CI/CD**

Доменное имя
```
    kit.blinklab.com
```
### 1.	Клонируйте репозиторий kittygram_final с проектом Kittygram со своего аккаунта на GitHub на локальный компьютер.

```
    git clone 'git@github.com:Dnevvs/kittygram_final.git'
```
### 2. Переходим в директорию проекта.

```
    cd kittygram_final/
```
### 3. Создаём виртуальное окружение.
```
    python -m venv venv
```
### 4. Активируем виртуальное окружение.
```
    source venv/scripts/activate
```
### 5. Устанавливаем зависимости.
```
    pip install -r requirements.txt
```
### 6. Переходим в директорию backend-приложения проекта.
```
	cd backend
```
### 7. Применяем миграции.
```
	python manage.py migrate
```
### 8.Создаём суперпользователя.
```
    python manage.py createsuperuser
```
### 9. Перейдите в директорию kittygram_final/frontend/ и выполните команду:
```
	npm i 
```
### 10.	Настройте веб-сервер Nginx для перенаправления запросов и работы со статикой проекта Kittygram: 
Опишите нужные настройки в существующем файле конфигурации.
```
    sudo nano /etc/nginx/sites-enabled/default 
```
```
server {
    server_name 130.193.53.253 kit.blinklab.com;

    location / {
        proxy_pass http://127.0.0.1:9000;
    }

    location /sentry-debug/ {
        proxy_pass http://127.0.0.1:9000;
    }
    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/firsttask.ddns.net/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/firsttask.ddns.net/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

}
server {
    if ($host = kit.blinklab.com) {
        return 301 https://$host$request_uri;
    }
    server_name 130.193.53.253 kit.blinklab.com;
    listen 80;
    return 404;
}
Перезапустите сервер:
```    
    sudo nginx -t
    sudo systemctl reload nginx 
```    ```
### 11. Подготовить папку kittygram на удаленном сервере.
На удаленном сервере создайте папку ```kittygram```.
В папку скопируйте файл ```.env``` из корневой папки проекта.
В папку скопируйте файл ```docker-compose.production.yml``` из корневой папки проекта.
### 12. Запуску деплоя на удаленный сервер:
На локальном компютере в папке kittygram_final допишите свой никнейм в данный файл README.md.
Затем последовательно выполните команды.
```
    git add .
    git commit -m 'Deploy'
    git push
```
На удаленном сервере автомтически развернется проект и будте доступен по доменному имени:
```
    kit.blinklab.com
```
Никнейм: Dnevvs
