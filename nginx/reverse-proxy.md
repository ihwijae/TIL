## 리버스 프록시 란?

클라이언트 요청을 대신 받아 내부 서버로 전달 해주는 것을 리버스 프록시라고 한다.  

`프록시: 대리라는 의미로, 정보를 대신 전달해주는 주체`

이 프록시 없이 웹 서버를 운영한다면  
localhost:3000 이라고 하는 웹서버를 운영한다면,  
웹 서버가 그대로 노출되기 때문에 보안상 위험하다.  포트번호도 그대로 노출된다.  

따라서 `사용자 -> nginx -> 웹서버` 로 구성해서 nginx가 대신 웹 서버로 전달해주는 역할을 하도록 구성해야한다.

<br>

## 리버스 프록시 장점
- 로드 밸런싱: Nginx는 클라이언트의 요청을 프록시 서버에 분산하기 위해 로드 밸런싱을 수행하여 성능, 확장성 및 신뢰성을 향상시킬 수 있다.


- 캐싱 : Nginx를 역방향 프록시로 사용하면 미리 렌더링된 버전의 페이지를 캐시하여 페이지 로드 시간을 단축할 수 있습니다. 이 기능은 프록시 서버의 응답에서 수신한 콘텐츠를 캐싱하고 이 콘텐츠를 사용하여 매번 동일한 콘텐츠를 프록시 서버에 연결할 필요 없이 클라이언트에 응답하는 방식으로 작동합니다.
  

- SSL 터미네이션 : Nginx는 클라이언트와의 연결에 대한 SSL 끝점 역할을 할 수 있습니다. 수신 SSL 연결을 처리 및 해독하고 프록시 서버의 응답을 암호화합니다.

- 압축 : 프록시 서버가 압축된 응답을 보내지 않는 경우 클라이언트로 보내기 전에 응답을 압축하도록 Nginx를 구성할 수 있습니다.


<br>


## 코드

#### default.conf
```
upstream backend {
        server workcongw-app:8080; 
        # docker 를 사용하지 않는다면 localhost:3000 (웹서버 주소)
        # docker 를 사용한다면 실행중인 웹 서버 컨테이너 이름 또는
        # docker 를 사용한다면 172.17.0.1
}

# http로 요청이 왔을때 서버 블록
server { 
    listen 80;
    server_name workcongw.store # 도메인 이름 또는 IP
    server_tokens off;



    location /.well-known/acme-challenge/ { # HTTP를 위한 SSL 인증서 설정
        allow all;
        root /var/www/certbot;
    }

     location / { # http로 요청이 왔을때 https로 리다이렉트
                return 301 https://$host$request_uri;
        }
}

# https로 요청이 왔을떄 서버블록.
server { 
        listen 443 ssl;
        server_name workcongw.store
        server_tokens off;

        ssl_certificate /etc/letsencrypt/live/workcongw.store/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/workcongw.store/privkey.pem;
        include /etc/letsencrypt/options-ssl-nginx.conf;
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;


        # workcongw.store 로 요청이 왔을때 로그인 폼으로 가기위한 리다이렉트 설정.
        location = / {
        return 301 /WorkConGW/common/loginForm;
         }

        # 모든 경로로 오는 요청을 upstream의 backend 경로로 포워딩.
         location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
         }



        }

        # www로 오는 요청을 www가 없는 경로로 리다이렉트를 위한 설정.
        server {
    listen 443 ssl;
    server_name www.workcongw.store;


    ssl_certificate /etc/letsencrypt/live/workcongw.store/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/workcongw.store/privkey.pem;

    return 301 https://workcongw.store$request_uri;
}


```


<br>

1. 사용자가 nginx 서버 주소로 요청한다 (workcongw.stroe)
2. nginx서버는 location 블록에서 proxy_pass로 지정된 주소로 요청을 전달해준다.(이때 웹 서버로 요청을 전달해주는것이다.)
3. 만약 운영중인 서버가 localhost:8080, 혹은 localhost:3000 이면 upstream 블록에 localhost:8080 혹은 localhost:3000 적어주면된다.

`upstream 블럭에 여러 서버를 명시해서 로드 밸런싱을 사용할 수 있다.`