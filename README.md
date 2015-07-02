Dockerfile - Codis
=================
#### - Run
```sh
root@ruo91:~# docker run -d --name="codis" -h "codis" \
-p 18087:18087 -p 11000:11000 -p 19000:19000 ruo91/codis
```
or

#### - Build
```sh
root@ruo91:~# docker build --rm -t codis https://github.com/ruo91/docker-codis.git
```

#### - Run
```
root@ruo91:~# docker run -d --name="codis" -h "codis" \
-p 18087:18087 -p 11000:11000 -p 19000:19000 codis
```

#### - Codis start
```sh
root@ruo91:~# docker exec codis /bin/bash codis-start --help
Usage: codis-start [COMMAND]

dashboard                       : Start dashboard
redis                           : Start redis
add_group                       : Add redis group
initslot                        : Init slot
proxy                           : Start proxy
-h, --h, --help                 : Show help
```

```sh
root@ruo91:~# docker exec -d codis /bin/bash codis-start all start
```

#### - Reverse proxy settings of Nginx
```sh
root@ruo91:~# nano /etc/nginx/nginx.conf
```

```sh
http {
.............
...................
.......................
# Codis dashboard
server {
    listen  80;
    server_name codis.yongbok.net;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://localhost:18087;
        client_max_body_size 10M;
    }
}
.......................
...................
.............
}
```

```sh
root@ruo91:~# nginx -t 
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@ruo91:~# nginx -s reload
```
Thanks. :-)