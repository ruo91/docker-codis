
# Dockerfile - Codis
#### - Run

    root@ruo91:~# docker run -d --name="codis" -h "codis" \
    -p 18087:18087 -p 11000:11000 -p 19000:19000 ruo91/codis

or

#### - Build

    root@ruo91:~# docker build --rm -t codis https://github.com/ruo91/docker-codis.git

#### - Run

    root@ruo91:~# docker run -d --name="codis" -h "codis" \
    -p 18087:18087 -p 11000:11000 -p 19000:19000 codis

#### - Codis start

    root@ruo91:~# docker exec codis /bin/bash codis-start --help
    Usage: codis-start [COMMAND] [start]
    
    dashboard                       : Start dashboard
    redis                           : Start redis
    add_group                       : Add redis group
    initslot                        : Init slot
    proxy                           : Start proxy
    -h, --h, --help                 : Show help
    
    root@ruo91:~# docker exec -d codis /bin/bash codis-start all start

#### - Reverse proxy settings of Nginx

    root@ruo91:~# nano /etc/nginx/nginx.conf
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
    root@ruo91:~# nginx -t 
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    root@ruo91:~# nginx -s reload

#### - Codis Dashboard
Overview
![Overview](http://cdn.yongbok.net/ruo91/img/docker/codis/codis_dashboard_0.png)

Server Groups
![Server Groups](http://cdn.yongbok.net/ruo91/img/docker/codis/codis_dashboard_1.png)

Slot Control & Migrate Status & Proxy Status
![Slot Control & Migrate Status & Proxy Status](http://cdn.yongbok.net/ruo91/img/docker/codis/codis_dashboard_2.png)

Thanks. :-)
