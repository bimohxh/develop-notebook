## Proxy Config

```
server{
      listen 80;
      server_name www.xxxx.com;
      location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://47.93.90.153:5000;
        proxy_redirect off;
      }
    }
```

## SSL

```
server {
  listen 443 ssl http2;
  server_name www.xxx.com;
  ssl on;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_certificate /etc/letsencrypt/live/xxx.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/xxx.com/privkey.pem;
  ssl_session_timeout 5m;
  ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
  ssl_prefer_server_ciphers on;
  location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://47.93.90.153:5000;
        proxy_redirect off;
      }
}

server {
  listen  80;
  server_name    xxx.com www.xxx.com;
  rewrite ^(.*)$  https://www.xxx.com$1 permanent;
}
```

## Letencrypt

generate ssl

```
 certbot certonly  -d *.xxx.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
 ```
 
 transport to sliyun oss privikey
 
 ```
 openssl rsa -in privkey.pem -out new_privkey.pem
 ```
