```
sudo mkdir -p /var/www/your_domain/html
```
```
sudo chown -R $USER:$USER /var/www/your_domain/html
```
```
sudo chmod -R 755 /var/www/your_domain
```
```
sudo nano /var/www/your_domain/html/index.html
```
```
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
</html>
```
```
sudo nano /etc/nginx/sites-available/your_domain
```
```
server{
	listen 80;
    listen [::]:80;

    index index.html server.js;

    server_name "domain or ip";

	 location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
```
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```
```
sudo nano /etc/nginx/nginx.conf
```
```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```
```
sudo nginx -t
```
```
sudo systemctl restart nginx
```

## DEPLOY FRONTEND (ReactJS)

sample nodejs

change file below
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

```
server {

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }

    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/your_domain/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/your_domain/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = www.your_domain) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = your_domain) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


        listen 80;
        listen [::]:80;

        server_name your_domain www.your_domain;
    return 404; # managed by Certbot
}
```

## BONUS Nginx Socket.IO

```
http {
  server {
    listen 80;

    location / {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $host;

      proxy_pass http://localhost:3000;

      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
    }
  }
}
```
