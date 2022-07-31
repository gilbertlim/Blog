---
title: "[Nginx] Nginx 사용법 (Mac OS)"
category: Nginx
---

<br><br><br>

Nginx를 MacOS에서 사용하는 방법을 간단히 알아보자.

# Nginx 설치

`brew install nginx`

# 주요 명령어

- nginx 데몬 실행 : `nginx`
- nginx 데몬 Reload : `nginx -s reload`
- nginx 데몬 종료 : `nginx -s stop`
- nginx PID 확인 : `ps -ef | grep nginx`
- nginx PID 삭제 : `sudo kill [PID]`

# 기초 사용법

Nginx config는 `/usr/local/etc/nginx/nginx.conf` 경로에 있고, config는 아래와 같이 수정한다.

`vi /usr/local/etc/nginx/nginx.conf`

```
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    server {
        listen       8080;
        server_name  localhost;

        access_log /usr/local/etc/nginx/access.log  main;
        error_log /usr/local/etc/nginx/error.log;

        location / {
            root   html;
            index  index.html index.htm;
        }
    }
}
```

`nginx -s reload` 로 변경된 config를 반영해주자.

<br>

브라우저를 통해 `localhost:8080`으로 들어가면, 다음과 같은 화면이 나타난다. 그러면 성공!

실무에서는 Postman과 같은 프로그램을 통해 API를 호출한다.

<img src="/assets/images/posts/devops/nginx.png" alt="" width="75%" />

`tail -f /usr/local/etc/nginx/access.log` 를 실행해보면 접속할 때마다 로그가 찍히게 됩니다.

<img src="/assets/images/posts/devops/nginx_log.png" alt="" width="75%" />