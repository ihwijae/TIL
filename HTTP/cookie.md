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