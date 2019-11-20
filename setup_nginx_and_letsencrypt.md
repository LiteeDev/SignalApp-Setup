## Install Nginx HTTP Server

```bash
  sudo apt update
  sudo apt-get remove httpd -y
  sudo apt install nginx -y
  sudo systemctl stop nginx.service
  sudo systemctl enable nginx.service
```

# Install PHP 7.2-FPM and Related Modules
```bash
  sudo apt-get install software-properties-common
  sudo add-apt-repository ppa:ondrej/php
  sudo apt update
  sudo apt install php7.2-fpm php7.2-common php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-intl php7.2-mysql php7.2-cli php7.2-zip php7.2-curl
```

# Create the vhost configuration for the domain.
```bash
  nano /etc/nginx/conf.d/domain.com.conf
```

# Do not forget to set DNS this important for the SSL.

```
server {
    listen 80;
    listen 443;
    server_name domain.com;

    ssl_certificate /etc/letsencrypt/live/domain.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/domain.com/privkey.pem; # managed by Certbot

    ssl on;
    ssl_session_cache  builtin:1000  shared:SSL:10m;
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
    ssl_prefer_server_ciphers on;

    access_log            /var/log/nginx/jenkins.access.log;

          location /v1/websocket {
                proxy_pass         http://localhost:8080/v1/websocket;
                proxy_http_version 1.1;
                proxy_set_header   Upgrade $http_upgrade;
                proxy_set_header   Connection "upgrade";
                proxy_set_header   Host $host;
          }


    location / {

      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;

      # Fix the “It appears that your reverse proxy set up is broken" error.
      proxy_pass          http://localhost:8080;
      proxy_read_timeout  90;
    }
  }

```

# Install letsencrypt and restart nginx.
```
  sudo add-apt-repository ppa:certbot/certbot
  sudo apt install python-certbot-apache
  sudo certbot certonly -d your_domain -d www.your_domain
```

Start a standalone server let the verification start and complete.

# Now, restart Nginx (If 502 gateway, restart Signal Server)


```
  service nginx restart
```
