# HTTP API 설계 예시
- HTTP API - 컬렉션
  - POST 기반 등록
  - 예) 회원 관리 API 제공
- HTTP API - 스토어
  - PUT 기반 등록
  - 예) 정적 컨텐츠 관리, 원격 파일 관리
- HTML FORM 사용
  - 웹 페이지 회원 관리
  - GET, POST만 지원

<br>

## 회원 관리 시스템 API 설계 예시(POST 기반)
- 회원 목록 /members -> `GET`
- 회원 등록 /members -> `POST`
- 회원 조회 /members/{id} -> `GET` (특정 회원만 조회)
- 회원 수정 /members/{id} -> `PATCH, PUT, POST`
- 회원 삭제 /members/{id} -> `DELETE`

<br>

### POST - 신규 자원 등록 특징
- 클라이언트는 등록될 리소스의 URI를 모른다
  - 회원 등록: /members -> `POST`
  - POST /members
- 서버가 새로 등록된 리소스 URI를 생성해줌
  -  HTTP/1.1 201 Created <br>
     Location: /members/100
- 컬렉션(Collection)
  - 서버가 관리하는 리소스 디렉토리
  - 서버가 리소스의 URI를 생성하고 관리
  - 여기서 컬렉션은 /members

<br>

> PUT 기반 등록은 파일 관리 시스템으로 예시 <br>
> 이유는, 파일 등록을 생각해보면 기존 파일을 지우고 다시 등록한다 <br>
> PUT은 기존파일이 있으면 덮어쓰기 때문에 이러한 예시와 잘맞다.

## 파일 관리 시스템 API 설계 예시(PUT 기반)
- 파일 목록 /files -> `GET`
- 파일 조회 /files/{filename} -> `GET` (특정 파일만 조회)
- 파일 등록 /files/{filename} -> `PUT`
- 파일 삭제 /files/{filename} -> `DELETE`
- 파일 대량 등록 /files -> `POST`

<br>

### PUT - 신규 자원 등록 특징
- 클라이언트가 리소스 URI를 알고 있어야 한다
  - 파일 등록: /files/{filename} -> `PUT`
  - PUT /files/star.jpg
- 클라이언트가 직접 리소스 URI를 지정한다. (서버는 클라이언트의 요청대로 등록함)
- 스토어(Store)
  - 클라이언트가 관리하는 리소스 저장소
  - 클라리언트가 리소스의 URI를 알고 관리
  - 여기서 스토어는 /files

<br>

> 대부분 POST 기반의 등록을 사용한다고 함 <br>
> 컬렉션을 대부분 사용한다.

<br>

## HTML FORM 사용
- HTML FORM은 `GET, POST만 지원`
- AJAX 같은 기술을 사용해서 해결 가능 -> 회원 API 참고
- 여기서는 순수 HTML, HTML FORM 이야기다
- GET, POST만 지원하므로 제약이 있다

<br>

#### API 설계 

- 회원 목록 /members -> `GET` 
- 회원 등록 폼 /members/new -> `GET` (폼 HTML페이지)
- 회원 등록 /members/new, /members -> `POST` (예를들어 저장버튼? 누르면 submit)
- 회원 조회 /members/{id} -> `GET`
- 회원 수정 폼 /members/{id}/edit -> `GET`
- 회원 수정 /members/{id}/edit, /members/{id} -> `POST`
- 회원 삭제 /members/{id}/delete -> `POST`

> 1. 회원 등록 할때 등록 폼 URI와 실제 저장버튼 URI를 동일하게 설계 가능하다 GET, POST로 구분 <br>
> 2. 회원을 저장하는건 컬렉션으로 자원을 관리하는것처럼 /members POST 로 사용해도 된다 <br>
> `1번 방식을 권장한다. (수정도 동일)`

`HTML FORM은 GET, POST만 지원하기 때문에 제약이 있다`

#### 컨트롤 URI
- GET, POST만 지원하는 제약을 해결하기 위해 동사로 된 리소스 경로 사용
- POST의 /new, /edit, /delete가 컨트롤 URI
- HTTP 메서드로 해결하기 애매한 경우 컨트롤 URI 사용 (스프링의 controller)

#### 정리
- HTTP API - 컬렉션
  - POST 기반 등록
  - 서버가 리소스 URI 결정
- HTTP API - 스토어
  - PUT 기반 등록
  - 클라이언트가 리소스 URI 결정
- HTML FORM 사용
  - 순수 HTML + HTML FORM 사용
  - GET, POST만 지원 (컨트롤 URI 사용)

<br>

## 참고하면 좋은 URI 설계 개념
- `문서(document)`
  - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
  - 예) /members/100, /files/star.jpg
- `컬렉션(collection)`
  - 서버가 관리하는 리소스 디렉터리
  - 서버가 리소스의 URI를 생성하고 관리
  - 예) /members
- `스토어(store)`
  - 클라이언트가 관리하는 자원 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - 예) /files
- `컨트롤러(controller), 컨트롤 URI`
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  - 동사를 직접 사용
  - 예) /members/{id}/delete


> `가능한 최대한 리소스만 가지고 설계한다. 기능은 HTTP API로 설계한다` <br>
> 즉, 문서, 컬렉션을 가지고 최대한 설계 한다. <br>
>  애매한경우 컨트롤 URI 사용한다