# HTTP 메서드 - PUT, PATCH, DELETE

## PUT
- 리소스를 대체 (PC에서 복사,붙여넣기를 생각하자 폴더에 파일이 없으면 생성되지만 있으면 지우고 새로운 파일로 덮어쓰기)
    - 리소스가 있으면 대체
    - 리소스가 없으면 생성
    - 쉽게 말하자면, 덮어버림
- `중요한점은 클라이언트가 리소스를 식별`
  - 클라이언트가 리소스 위치를 알고 URI를 지정
  - POST와 차이점 (POST는 서버에서 리소스 식별)

`리소스가 있는 경우 예시`
```angular2html
    /members/100 리소스
{
    "username": young
    "age": 20
}
```
<br>

```angular2html
PUT /members/100 리소스에 PUT 요청

PUT /members/100 HTTP/1.1
Context-Type: application/json

{
    "username": "old",
    "age": 50
}
```

<br>

기존 리소스가 이미 있으니 새로운 리로스로 대체 된다.
```angular2html
/members/100 리소스
{
    "username": "old",
    "age": 50
}
```

<br>

`리소스가 없는 경우 예시`
```angular2html
PUT /members/100 리소스에 PUT 요청

PUT /members/100 HTTP/1.1
Context-Type: application/json

{
    "username": "old",
    "age": 50
}
```

<br>

```angular2html
/members/100 리소스
{
"username": "old",
"age": 50
}
```

<br>

`주의할 점: 리소스를 완전히 대체한다`
```angular2html
/members/100 기존 리소스
{
"username": "old"
"age": 50
{
```

<br>


```angular2html
PUT /members/100 HTTP/1.1
Context-Type: application/json

{
    "age": 50
}
```

<br>

```angular2html
/members/100 리소스 대체

{
"age": 50
}
```

<br>

기존 리소스를 완전히 대체해서 username 필드가 삭제됨

부분수정을 하려면 PATCH 메서드를 사용하면 된다

<br>

## PATCH
- 리소스 부분 변경

```angular2html
/members/100 기존 리소스

{
    "username": "young"
    "age": 20
}
```

<br>

```angular2html
/members/100 PATHCH 요청

PATCH /members/100 HTTP/1.1
Context-Type: application/json

{
    "age": 50
}
```

<br>

```angular2html
/members/100 수정된 리소스

{
    "username": "young"
    "age": 50
}
```
부분적으로 age 필드만 50으로 변경됐다

<br>


## DELETE
- 리소스 삭제
```angular2html
DELETE /members/100 HTTP/1.1
Host: localhost:8080
```
/members/100에 리소스가 삭제됨
