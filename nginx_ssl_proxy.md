
# IPv4 to IPv6 SSL reverse proxy with Nginx

When you have HTTP services running on IPv6 addresses, and you want make it accessible from IPv4 space.
Schema is like this:  
`<client> --IPv4 https-- 443<nginx> --IPv6 http-- 80<service>`

```
apt install nginx
snap install core
snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
```




```
server {
    server_name grafana.example.com;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP  $remote_addr;
        proxy_pass http://[2a02:598:a::79:222];
    }

    listen 192.168.10.11:443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/grafana.example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/grafana.releaseregister.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
```

Sources:  
- https://letsencrypt.org/getting-started/
- https://certbot.eff.org/
- https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal
