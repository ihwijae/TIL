# 2xx - 성공

<br>

## 2xx (Successful)
#### 클라이언트 요청을 성공적으로 처리
- 200 OK
- 201 Created
- 202 Accepted
- 204 No Content

<br>

## 200 OK
#### 요청 성공
```angular2html
GET /members/100 HTTP/1.1
Host: localhost:8080
```

클라이언트에서 요청

```angular2html
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 34

{
    "username": "young"
    "age": 20
}
```

서버에서 결과를 정상적으로 처리해서 응답 HTTP 메시지 스타트 라인에 200 OK

<br>

## 201 Created
#### 요청 성공해서 새로운 리소스가 생성 됐다

```angular2html
POST /members HTTP/1.1
Content-Type: application/json

{
"username": "young",
"age": 20
}
```

클라아언트 요청

```angular2html
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 34
Location: /members/100

{
"username": "young",
"age": 20
}
```

`생성된 리소스는 응답의 Location 헤더 필드로 식별` <br>
`201 Created를 보고 이 메시지는 Location 필더가 있겠다 판단 가능`

## 202 Accepted
#### 요청이 접수되었으나 처리가 완료되지 않았음
- 배치 처리 같은 곳에서 사용
- 예) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함

잘 사용하진 않는다.

<br>

## 204 No Content
#### 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
- 얘) 웹 문서 편집기에서 save 버튼
- save 버튼의 결과로 아무 내용이 없어도 된다
- save 버튼을 눌러도 같은 화면을 유지해야 한다
- 결과 내용이 없어도 204메시지(2xx) 만으로 성공을 인식할 수 있다