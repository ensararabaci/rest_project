Bu bir Rest API ödevidir. 

1. Rest Api Kullanımı: http://127.0.0.1/users/5 (Rest api portu 80 portuna ayarlanmıştır. Farklı bir port kullanılırsa URL değişmelidir.)

2. NGINX Conf Dosyası:
Conf Path: /etc/nginx/sites-enabled/ensarhttp

Conf Text:
```
server {
    listen 82;
    location / {
        include proxy_params;
        proxy_pass http://unix:/home/ubuntu/odev/ensarproject.sock;
    }
}
```


3. GUNICORN Komutu: /home/ubuntu/odev/ensarprojectenv/bin/gunicorn --workers 3 --bind unix:ensarproject.sock -m 007 wsgi:app

4. Ubuntu System Service:
Service Path: /etc/systemd/system/ensarservice.service

Service Text:
```
[Unit]
Description=Gunicorn ENSAR
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/odev
Environment="PATH=/home/ubuntu/odev/ensarprojectenv/bin"
ExecStart=/home/ubuntu/odev/ensarprojectenv/bin/gunicorn --workers 3 --bind unix:ensarproject.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

