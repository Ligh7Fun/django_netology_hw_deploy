<details>
  <summary>Создание пользователя</summary>

Выполняем команду `adduser denis` для создания пользователя.

Добавляем в кгруппу `usermod denis -aG sudo`

Меняем пользователя `su denis`
  
</details>

<details>
  <summary>Создаем ssh ключ и алиас</summary>

Создаем ключ на своем пк командой `ssh-keygen -t rsa`
    
Копируем его на удаленный сервер командой `sh-copy-id -i ~/.ssh/id_rsa.pub user@server` заменив user и server на свои данные.

Настраиваем на своем пк `~/.ssh/config` добавив:
```
Host netology_hw
    HostName 79.174.83.158
    User denis
```

При выполнении команды `ssh netology_hw` происходит подключение к удаленному серверу без ввода пароля.

</details>

   
<details>
  <summary>Установка пакетов</summary>

```python
pip install Django gunicorn
```
  
</details>   

<details>
  <summary>Структура проекта</summary>

```
(env) root@cv3411647:~/project# tree -L 2
.
├── env
│   ├── bin
│   ├── include
│   ├── lib
│   ├── lib64 -> lib
│   └── pyvenv.cfg
└── promo
    ├── db.sqlite3
    ├── landing
    ├── manage.py
    ├── promo
    ├── promo.sock
    └── static
```
  
</details>

<details>
  <summary>Структура конфига nginx</summary>

```nginx
server {
  listen 80;
  server_name 79.174.83.158;

  location /static/ {
    root /root/project/promo;
  }

  location / {
    include proxy_params;
    proxy_pass http://unix:/root/project/promo/promo.sock;
  }
}
```
  
</details>

<details>
  <summary>Структура gunicorn.service</summary>

```
[Unit]
Description=promo landing
After=network.target

[Service]
User=root
Group=www-data
WorkingDirectory=/root/project/promo
ExecStart=/root/project/env/bin/gunicorn --workers 3 -b unix:/root/project/promo/promo.sock promo.wsgi

[Install]
WantedBy=multi-user.target
```
  
</details>
