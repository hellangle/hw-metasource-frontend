# Repositories

Backend: https://github.com/hellangle/hw-metasource-backend
Frondend: https://github.com/hellangle/hw-metasource-frontend

# Development:

Backend:
> docker-compose up

Frondend:
> npx http-server

# Production

## Backend:
> push code to main branch

Endpoint: 
> GET https://hw.nhadavina.com/api-docs

> POST https://hw.nhadavina.com/signup

> GET https://hw.nhadavina.com/users

> GET https://hw.nhadavina.com/users/*id*

API Token:
Use Basic authentication
> username/password: metasource/testing


## Frondend:
> push code to main branch

Endpoint:
> https://d2rukhgml8h3yf.cloudfront.net

# Setup AWS
> username/password: metasource/123qwe!@#

## Backend:
Nginx & Mysql server run on EC2 <br/>
App run on docker on EC2
```
http {
	sendfile on;
	tcp_nopush on;
	types_hash_max_size 2048;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	upstream api-server {
    server localhost:3000;
    keepalive 100;
  }

  server {
    server_name hw.nhadavina.com;

    location / {
      proxy_http_version 1.1;
      proxy_pass http://api-server;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/hw.nhadavina.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/hw.nhadavina.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
  }
	
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;
	
	log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	gzip on;

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;

  server {
    if ($host = hw.nhadavina.com) {
      return 301 https://$host$request_uri;
    }

    listen 80;
    server_name hw.nhadavina.com;
    return 404; # managed by Certbot
  }
}
```
## Frontend:
CloudFront & S3
