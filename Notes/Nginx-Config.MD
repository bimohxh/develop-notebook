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

## Letencrypt

generate ssl

```
 certbot certonly  -d *.xxx.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
 ```
 
 transport to sliyun oss privikey
 
 ```
 openssl rsa -in privkey.pem -out new_privkey.pem
 ```
