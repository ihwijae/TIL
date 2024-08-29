## 웹 브라우저 요청 흐름

https://www.google.com:443/search?q=hello&hl=ko 이렇게 요청했다고 가정해본다.
* DNS 서버를 조회 (DNS 서버에서 IP주소를 반환해줌)
* HTTPS 포트 조회 (생략하면 443)
* 이렇게 하면 IP, PORT 정보 습득
* 웹 브라우저에서 HTTP 요청 메시지 생성

```angular2html
HTTP 요청 메세지
GET /search?q=hello&hi=ko HTTP/1.1
Host: www.google.com
```
1. 웹 브라우저가 HTTP 메시지 생성
2. socket 라이브러리를 통해 전달
   * A : TCP/IP 연결 (IP,PORT정보를 토대로 3 way handshake)
   * B : 데이터 전달 (데이터를 전송하기위해)
3. TPC/IP 패킷 생성 (HTTP 메세지 포함)
4. 네트워크 인터페이스를 통해 (LAN 카드) 서버에 패킷 전송

구글 서버에서는 TCP/IP 패킷을 까서 버리고 HTTP 메세지를 꺼내서 해석함.

요청 메세지를 토대로 데이터를 찾고, 응답 패킷 생성
```angular2html
HTTP 응답 메세지

HTTP/1.1 200 OK
Context-Type:text/html;charset=UTF-8
Context-Length : 3423

<html>
    <body>....</body>
</html>
```
클라이언트는 응답 패킷을 받고 응답 메시지에서 타입이 html이라면  html을 화면에 렌더링해서 결과를 볼 수 있다.