## 리다이렉트 흐름

```
 server {
        listen 443 ssl;
        server_name workcongw.store
        server_tokens off;

        ssl_certificate 
        /etc/letsencrypt/live/workcongw.store/fullchain.pem;

        ssl_certificate_key /etc/letsencrypt/live/workcongw.store/privkey.pem;

        include /etc/letsencrypt/options-ssl-nginx.conf;
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

        location = / {
        return 301 /WorkConGW/common/loginForm;
         }

         location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
         }



        }
```

#### location = / {} 
- 이 블록은 server_name에 설정된 도메인 루트 경로 (/)를 가리킨다.
- 지금은 server_name 이 workcongw.store로 설정 되있으니까
- 해당 블록의 경로는  workcongw.store 이게 되는것이다.

### location / {}
- 이 블록은 모든 경로에 대해 적용되는 기본 처리 블록이다.

<br>

이렇게 세팅 했다고 쳤을 때,  
흐름을 살펴보면,  

1. 클라이언트 -> 서버에 workcongw.stre 로 요청을 한다
2. location = / {}  이 블록이 실행되며 /WorkConGW/common/loginForm; 로 리다이렉트
3. 클라이언트는 리다이렉트 된 경로로 서버에 재 요청한다.
4. location / {} 블록이 실행되고 proxy_pass로 프록시 서버로 요청이 전송된다.

