# HTTP 메서드 - GET, POST

## HTTP 주요 메서드 종류
- GET : 리소스 조회
- POST : 요청 데이터 처리, 주로 등록에 사용
- PUT : 리소스르 대체, 해당 리소스가 없다면 생성
- PATCH : 리소스 부분 변경
- DELETE : 리소스 삭제

<br>

## HTTP 기타 메서드 종류
- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환 (바디 빼고 헤더까지 전부)
- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
- CONNECT: 대상 리소스로 식별되는 서버에 대한 터널을 설정
- TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

<br>

## GET
- 리소스 조회
- 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)을 통해서 전달
- 메시지 바디를 사용해서 데이터를 전달할 수 도있지만 지원하지 않는 곳이 많아서 권장되는 방법은 아니다.
```angular2html
GET 요청 메시지

GET /members/100 HTTP/1.1 (members에 id가 100인 리소스를 조회하는 요청)
Host: localhost:8080
```

<br>

```angular2html
GET 응답 메시지 (요청이 들어오고 맞는 데이터가 json 이라고 가정)

HTTP1.1/ 200 OK
Context-Type: application/json
Content-Length: 34

{
    "username": "lee"
    "age": 24
}
```
<br>

## POST
- 요청 데이터 처리
- `메시지 바디를 통해 서버로 데이터 전달`
- 서버는 요청 데이터를 처리
  - 메시지 바디를 통해 받은 데이터를 처리하는 모든 기능을 수행
- 주로 전달된 데이터는 신규 리소스 등록, 프로세스 처리에 사용
```angular2html 
POST 요청 메시지

POST /members HTTP/1.1
Context-Type: application/json

{
    "username": "young",
    "age": 20
}
```

<br>

```angular2html
POST 응답 메시지

/members로 요청이 왔으면 서버에서 신규 리소스 식별자 생성 -> /members/100

HTTP/1.1 201 Created
Context-Type: application/json
Content-Length: 34
Location: /members/100 (리소스가 생성된 URI 경로)

{
    "username": "young",
    "age": 20
}
```

<br>

## POST 요청 데이터를 어떻게 처리한다는 뜻일까? 예시
- 스펙: POST 메서드는 `대상 리소스가 리소스의 고유 한 의미 체계에 따라 요청에 포함 된 표현을 처리하도록 요청합니다.` (구글 번역)
- 예를 들어 POST는 다음과 같은 기능에 사용됩니다.
- HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
- 예) HTML FORM에 입력한 정보로 회원 가입, 주문 등에서 사용
- 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
- 예) 게시판 글쓰기, 댓글 달기
- 서버가 아직 식별하지 않은 새 리소스 생성
- 예) 신규 주문 생성
- 기존 자원에 데이터 추가
- 예) 한 문서 끝에 내용 추가하기
- `정리: 이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함` -> 정해진 것이 없음 (개발자가 POST 요청이오면 내부에서 어떻게 처리할지 정한다는 것)

<br>


## 🔍 POST 정리
1. 새 리소스 생성(등록)
   - 서버가 아직 식별되지 않은 새 리소스 생성
2. 요청 데이터 처리
    - 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는 경우
    - 예) 주문에서 결제완료 -> 배송시작 -> 배송완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우
    - POST의 결과로 새로운 리소스가 생성되지 않을 수도 있음 (서버에서 POST 요청이 수정,삭제가 되도록 설계도 가능하다)
    - 예) POST /orders/{orderId}/start-delivery `(컨트롤 URI)` -> 리소스만으로 설계가 안되는 경우 컨트롤 URI사용
3. 다른 메서드로 처리하기 애매한 경우
    - 예) 클라이언트에서 JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우 (위에 나왔듯이, JSON은 메세지 바디에 데이터를 담는다. 하지만 GET메서드는 메세지 바디를 잘 허용하지 않음)
    - 애매하면 POST 메서드 사용
    - `하지만 조회할땐 GET을 사용하는것이 좋다 GET메서드로 오면 캐싱이 쉽지만 POST는 캐싱 처리가 어렵다 `





