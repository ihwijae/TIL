# HTTP 메시지

![image](/HTTP/images/message.png)

## 요청 메시지 시작라인
- start-line =  `request-line(요청메시지)` / status-line(응답메시지)
- `request-line` = method SP(공백.스페이스바) request-target SP HTTP-version CRLF(엔터)

<br>


## 요청 메시지 시작라인 - HTTP 메서드
- 종류 : GET, POST, PUT, DELETE ...
- 서버가 수행해야 할 동작 지정
  - GET : 리소스 조회
  - POST : 요청 내역 처리(등록을 많이 씀)

<br>


## 요청 메시지 시작라인 - 요청 대상 (/search?p=hello&hl=ko)
- absolute-path[?query] (절대경로[?query])
- 절대경로= "/" 로 시작하는 경로
- 참고: *, http://...?x=y 와 같이 다른 유형의 경로지정 방법도 있다.


<br>
  
## 요청 메시지 시작라인 - HTTP 버전
- HTTP version

<br>


## 응답 메시지 시작라인
- start-line =  request-line(요청메시지) / `status-line(응답메시지)`
- `status-line` = HTTP-version SP status-code SP reason-phrase CRLF
  - HTTP 버전
  - HTTP 상태 코드 : 요청 성공, 실패를 나타냄
    - 200 : 성공
    - 400 : 클라이언트 요청 오류
    - 500 : 서버 내부 오류
  - 이유 문구 : 사람이 이해할 수 있는 짧은 상태코드 설명 글
  
  <br>

## HTTP 헤더
- header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 허용)
- field-name은 대소문자 구문 없음
  - 요청 메시지 : Host: www.google.com
  - 응답 메시지 : Content-Type: text/html;charset=UTF-8
    Content-Length: 3423

<br>

### 용도
- HTTP 전송에 필요한 모든 부가정보
- 예 ) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저)
-  표준 헤더가 너무 많음
-  필요시 임의의 헤더 추가 가능
-  예 ) helloworld: hihi

<br>

## HTTP 메시지 바디
### 용도
- 실제 전송할 데이터
- HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능

`HTTP는 단순하고 HTTP 메시지도 매우 단순하다. 단순하지만 확장 가능한 기술이다.`

<br>


### HTTP 정리
- HTTP 메시지에 모든 것을 전송
- HTTP 역사 HTTP/1.1을 기준으로 학습
- 클라이언트 서버 구조
- 무상태 프로토콜
- HTTP 메시지
- 단순함 , 확장 가능
- 지금은 HTTP의 시대