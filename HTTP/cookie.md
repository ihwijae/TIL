# 인증
- Authorization: 클라이언트 인증 정보를 서버에 전달
- WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의

## Authorization
#### 클라이언트 인증 정보를 서버에 전달
- Authorization: Basic xxxxxxxxx (여기에 인증과 관련된 값을 넣는다)

`인증과 관련해서 여러가지 메커니즘이 있다 OAuth 인증, 등등..`  
`인증 종류별로 벨류에 들어가는 값은 다르다 각 종류별로 벨류에 어떤 값을 넣어야하는지 찾아보면 된다.`


## WWW-Authenticate
#### 리소스 접근시 필요한 인증 방법 정의
- 리소스 접근 시 필요한 인증 방법 정의
- 401 Unauthorized 응답과 함께 사용 (로그인이 안된 등의 경우)
- WWW-Authenticate: Newauth realm="apps", type=1,
title="Login to \"apps\"", Basic realm="simple"


<br>


## 쿠키

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고 HTTP 요청시 헤더에 담아서 서버로 전달


<br>

## 쿠키 미사용 - 로그인

클라이언트
```
POST /login HTTP/1.1
user=이동글
```


서버
```
HTTP/1.1 200 OK

이동글 님이 로그인 했습니다
```

클라이언트가 유저 정보를 보내면 서버에서 로그인 된 HTMl 응답 (예시)  
실제로는 user뿐 아니라 password 같은 정보도 보내야 함.


#### 로그인 이후 사용자가 welcome 페이지 접근 한다면?

클라이언트
```
GET /welcome HTTP/1.1
```

서버
```
HTTP/1.1 200 OK
안녕하세요 손님.
```

난 이동글로 로그인 했고 이동글로 요청했지만  
서버는 이동글로 요청이 온건지 아닌지 판단 불가능하다.  
그래서 welcome 페이지에 `안녕하세요 이동글`님이 아닌 `안녕하세요 손님`이 나온다.  

#### HTTP는 무상태 프로토콜(stateless) 을 기억하자.
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다. (실제로는 지속연결이라 어느정도 지속 되긴 함)
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
- 클라이언트와 서버는 서로 상태를 유지하지 않는 무상태.


#### 쿠키 미사용시 로그인을 유지하는 대안

**간단하다 모든 요청에 사용자 정보를 포함해서 요청하면 된다.**

```
GET /welcome?user=홍길동 HTTP/1.1
```

서버
```
HTTP/1.1 200 OK
안녕하세요. 이동글님
```

하지만 이렇게 매번 요청시에 쿼리스트링 담아서 보내기는 개발하기가 너무 불편하다.  
/welcome만 아니라 모든 요청 url에도  다 담다야 한다는것이다.  
개발자가 모든 요청에 사용자정보를 포함하도록 개발해야 한다는것 (어렵다).  


## 쿠키 사용 
클라이언트 
```
POST /login HTTP/1.1
user=이동글
```

서버
```
HTTP/1.1 200 OK
Set-Cookie: user=홍길동

홍길동 님이 로그인 했습니다.
```

서버가 Ser-Cookie에 user=이동글 이란 정보를 쿠키에 담아서 응답해준다.  
쿠키를 받은 웹 브라우저 내부에는 쿠키 저장소가 존재한다.  
쿠키 저장소에 user=이동글 이란 값을 저장한다.


#### 로그인 이후 welcome 접근

클라이언트
```
GET /welcome HTTP/1.1
Cookie: user=이동글
```

서버
```
HTTP/1.1 200 OK
안녕하세요 이동글님
```

웹 브라우저에서 서버에 요청 할 때마다 쿠키저장소에서 쿠키를 찾는다.  
쿠키 값을 꺼내서 요청시에 헤더에 추가해서 보낸다.  
서버쪽에서도 쿠키 정보를 보고 user가 누군지 알 수 있다.  
지저분하게 url에 넣을 필요가 없다  

`쿠키는 모든 요청에 쿠키 정보 자동 포함`

## 쿠키 설명
- set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure (서버에서 설정)
- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 추가 트래픽 발생
  - 최소한의 정보만 사용할것(세션 id, 인증토큰(OAuth)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 저장하고 싶으면 웹 스토리지 사용  
`주의: 보안에 민감한 주민번호, 카드정보 등등은 저장하면 안된다`  


실제로 서버에서는 Set-Cookie: 에 user=홍길동 처럼 그대로 내려주는건 위험하다.  
서버 쪽에서는 로그인이 성공되면 sessionKey를 만들어서 서버에 데이터베이스에 저장한 후에,   
세션id를 클라이언트에게 반환해준다.  
sessionId=<서버에서 생선한 sessionKey>;  

클라이언트는 서버에 요청할때마다 sessionId를 보내고 서버에서는 sessionId를 보고 사용자가 누구인지 알 수 있는것.


## 쿠키 생명 주기
#### Expires, max-age
- Set-Cookie: expires=Sat, 26-Dec-2023 04:39:22 GMT
  - 만료일이 되면 쿠키 삭제
- Set-Cookie: max-age=3600 (3600초)
  - max-age에 0이나 음수를 적으면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지.

## 쿠키 - 도메인
#### Domain
- 예) domain=example.org
- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
  - domain=example.org를 지정해서 쿠키 생성하면
  - example.org는 물론이고 dev.example.org도 쿠키 접근 가능
- 생략: 현재 문서 기준 도메인만 적용
  - example.org에서 쿠키를 생성하는데 domain 지정을 생략하면?
    - example.org 에서만 쿠키 접근 가능
    - dev.example.org는 쿠키 미접근

## 쿠키 - 경로
#### Path
**예) path=/main**
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근 가능
- 일반적으로 path=/ 루트로 지정 (한 도메인안에서 보통 쿠키를 전부 전송하는 경우가 많음)
- 예) path=/main
  - /main -> 가능
  - /main/level1 -> 가능
  - /main/level1/lever2 -> 가능
  - /home -> 불가능


## 쿠키 - 보안
#### Secure, HttpOnly, SamsSite

- Secure
    - 쿠키는 http, https를 구분하지 않고 전송
    - Secure를 적용하면 https인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP 전송에만 사용
- SameSite
    - XSRF 공격 방지
    - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송