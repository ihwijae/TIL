# 모든것이 HTTP

### HTTP(Hyper Text Transfer Protocol)

## HTML을 전송하는 프로토콜로 시작 됐지만 지금은 모든 것을 전송할 수 있다.
* HTML, TEXT
* Image, 음성, 영상, 파일
* JSON, XML (API)
* 거의 모든 형태 데이터 전송 가능
* 서버간의 데이터 주고 받을 때도 대부분 HTTP 사용함


## HTTP 역사
* HTTP/0.9 1991년 : GET 메서드만 지원 HTTP 헤더 X
* HTTP/1.0 1996년 : 메서드 , 헤더 추가
* HTTP/1.1 1997년 : 가장 많이 사용, 우리에게 가장 중요한 버전 RFC2068(1997) > RFC2616 (1999) > RFC7230~7235(2014)
* HTTP/2 2015년 : 성능 대선
* HTTP/3 진행중 : TCP 대신에 UDP, 성능 개선

`HTTP/1.1이 가장 많이 사용되므로 이걸 위주로 공부하자.`

## 기반 프로토콜
* TCP : HTTP/1.1, HTTP/2
* UDP : HTTP/3
* 현재 HTTP/1.1 주로 사용
  * HTTP/2, HTTP/3 도 점점 증가하는 추세

## HTTP 특징
* 클라이언트 - 서버 구조
* 무상태 프로토콜(스테이트리스), 비 연결성
* HTTP 메시지로 구성
* 단순함, 확장 가능