# 일반 정보

- From: 유저 에이전트의 이메일 정보
- Referer: 이전 웹 페이지 주소
- User-Agent: 유저 에이전트 애플리케이션 정보
- Server: 요청을 처리하는 Origin 서버의 소프트웨어 정보
- Date: 메시지가 생성된 날짜



## FROM
#### 유저 에이전트의 이메일 정보
- 일반적으로 잘 사용하지 않는다.
- 검색 엔진 같은 곳에서 주로 사용
- 요청에서 사용

> 검색엔진 같은 곳에서 내 사이트를 크롤링 한다고 쳤을 때, 담당자한테 거부 의사를 말할때 사용하는 등.  
> From 필드를 보고 직접 담당자에게 연락을 해야한다.



## Referer
#### 이전 웹 페이지 주소
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A -> B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청한다.
- Referer를 사용해서 유입 경로 분석 가능
- 요청에서 사용
- 참고: Referer는 단어 오타 Referrer의 오타다.
  - 과거에 HTTP에서 이미 나간것이라 고칠 수 없다.
  - 고친 순간 Referer를 서버가 못 받아들인다.


> 예를들어 google.com 에서 위키백과로 들어간다고 쳤을 때,  
> 위키백과에서 보는 Referer은 google.com 이 된다.

`내 웹사이트를 어떤 경로로 들어왔는가 데이터 분석할때 많이 사용한다.`



## User-Agent
#### 유저 에이전트 애플리케이션 정보

- user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
- 클라이언트 애플리케이션 정보(웹 브라우저 정보, OS 등등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생했는지 파악 가능
- 요청에서 사용

`특정 웹 브라우저에서 버그가 생긴다면, 로그 파싱해보면 알 수 있다`


## Server
#### 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보

- server: Apache/2.22(Debian)
- server: nginx
- 응답에서 사용

HTTP 요청을 보내면 중간에 여러 프록시 서버를 거친다.   
이 프록시 서버를 거쳐서 실제 나의 요청을 처리해주는 응답해주는 마지막 서버를 ORIGIN 서버라고 한다. (표현 데이터를 만들어주는 서버)

## Date
#### 메시지가 발생한 날짜와 시간
- Date: Tue, 15 Nov 1994 08:12:31 GMT
- 응답에서 사용 (과거에서는 요청에서도 사용했지만, 최신 스펙에서는 응답에만 사용)

<br>


# 특별한 정보
- Host: 요청한 호스트 정보(도메인)
- Location: 페이지 리다이렉션
- Allow: 허용 가능한 HTTP 메서드
- Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

<br>

## Host
#### 요청한 호스트 정보(도메인)

- 요청에서 사용
- 필수 헤더
- 하나의 서버가 여러 도메인을 처리해야 할 때
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

```
Host: www.google.com
```

![image](./images/image%20copy%204.png)
 
`가상호스트는 포트와는 무관하다.`  
`호스트를 먼저 찾고 그 안에서 포트로 구분한다고 이해하면 된다.`  
`같은ip, 같은포트에 서로다른 도메인이 사용 가능하다 (단 http(80), https(443만)`  
`해당 호스트로 요청이 오면 웹 서버에서 정의한 내용대로 맞는 어플리케이션 포트로 포트 포워딩 한다.`  
`당연한 얘기지만 같은 포트로 여러 어플리케이션을 동시에 사용할 수는 없다 도메인만 가능하다`

nginx config 설정
```
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:8080;
    }
}

server {
    listen 80;
    server_name example.org;

    location / {
        proxy_pass http://localhost:8081;
    }
}
```
- example.com으로 오는 요청은 localhost:8080으로, example.org로 오는 요청은 localhost:8081으로 전달됩니다.  
- 동일한 IP와 포트(80번)를 사용하지만, 도메인 이름에 따라 다른 서버 블록이 선택되어 처리되는 것이다.


#### 여기서 Host필드가 없다면 생기는 문제.
![image](./images/image%20copy%205.png)

- 가상호스팅이란, 도메인을 보고 분기해서 처리하는 기능이다.  

<br>

이렇게 클라이언트가 Host없이 요청을 보내면  
같은 ip에서 여러 도메인을 사용하는 서버 입장에서는 어떤 도메인으로 연결된 어플리케이션인지 구분 할 수가 없다.   
그래서 요청시에 Host명을 적어줘야 서버에서 구분이 가능하다.  
서버는 먼저 Host(도메인)을 찾고 그 안에서 다시 port로 구분한다고 생각하면 된다.  
예를들어 443동일한 포트에 www.aaa.com, www.bbb.com 이렇게 서로 다른 도메인을 사용할 수 있는것이다.  
Nginx를 사용하면 443 동일포트로 www.aaa.com 혹은 www.bbb.com로 요청이 오면 해당 맞는 어플리케이션으로 포트 포워딩이 가능하기 때문이다.

1. Apache, Nginx 같은 웹 서버가 80번 포트에서 대기한다
2. 자바 웹앱은 각각의 포트에서 실행 된다.
3. 사용자 요청이 들어올 경우 도메인 정보에 따라 그에 맞는 자바 웹앱으로, web server가 포트 포워딩한다.
4. web server가 이런 기능을 제공하고 있고, 설정 정보에는 자바 웹앱의 포트정보가 포함 되어 있어야 한다.


![image](./images/image%20copy%206.png)

요청시에 Host 헤더를 사용해서 어떤 도메인으로 요청할것인지 명시해준다.  
서버에서는 Host 헤더에 맞는 도메인에 연결해준다.  



## Location
#### 페이지 리다이렉션
- 웹 브라우저는 3xx 응답의 결과에 location 헤더가 있으면, location위치로 자동 이동한다. (리다이렉트)
- 201 (created): Location 값은 요청에 의해 새로 생성된 리소스 URI
- 3xx(Redirection): Location 값은 요청을 자동으로 리다이렉션 하기 위한 대상 리소르를 가리킨다.

## Allodw
#### 허용 가능한 HTTP 메서드
- 405(Method Not Allowed) 에서 응답에 포함해야한다.
- Allow: GET, HEAD, PUT (해당 서버는 GET, HEAD, PUT메서드만 사용가능하다)

클라이언트가 DELETE로 요청이 왔는데 서버에서 지원하지 않는 메서드라서 Allodw로 지원 가능한 메서드를 명시해서 응답해준다.


## Retry-Aftet
#### 유저 에이전트가 다음 요청을 하기까지 기다려야 하는시간
- 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있다
- Retry-After: Fri, 31 Dec 1999 23:59:23 GMT (날짜표기)
- Retry-After: 120 (초단위 표기)