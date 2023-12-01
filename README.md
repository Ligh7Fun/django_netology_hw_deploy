Установка пакетов:

`pip install Django gunicorn`

Структура проекта:

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

Структура конфига nginx:
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

Структура gunicorn.service:
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

