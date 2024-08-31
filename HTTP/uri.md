## URI (Uniform Resource Identifier)
* Resource를 식별하는 통합된 방법
* 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류 할 수 있다.
* URI가 가장 큰 개념안에 URL, URN이 있다

`URL(Uniform Resource Locator) : 리소스의 위치`
`URN(Uniform Resource Name) : 리소스의 이름`

![image](/HTTP/images/uri.png)
보통 URL을 주로 사용한다.


<br>

## URI 단어 뜻
* Uniform : 리소르를 식별하는 통일된 방식
* Resource : 자원, URI로 식별할 수 있는 모든것(제한없음)
* Identifier : 다른 항목과 구분하는데 필요한 정보 (사람을 식별한다고하면, 주민번호로 식별한다 치면 주민번호가 Identifier)

<br>

## URL, URN 뜻
* URL - Locator: 리소스가 있는 위치를 지정
* URN - Name : 리소스에 이름을 부여
* 위치는 변할 수 있지만 이름은 변하지 않는다.
* URN 이름 만으로 실제 리소르를 찾는 방법은 보편화 되지 않음

<br>


## URL을 분석해보자
https://www.google.com/search?q=hello&hl=ko

#### 전체 문법
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* 프로토콜(https)
* 호스트명(www.google.com)
* 포트번호 : 443
* 패스 (/search)
* 쿼리 파라미터 (q=hello&hi=ko)

#### scheme
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* 주로 프로토콜 사용
* 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 클라이언트와 서버간의 약속과 규칙
  * 예) http, https, ftp 등등...
* http는 80포트 https는 443 포트를 주로 사용. 포트번호는 생략 가능.
* https는 http에서 보안 기능 추가된 것 

#### userinfo
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* URL에 사용자 정보를 포함해서 인증해야 할때 사용
* 거의 사용하지 않음

#### Host
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* 호스트명
* 도메인명 또는 IP주소를 직접 입력할 수 있다

#### PORT
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* 포트 (PORT)
* 접속 포트
* 일반적으로 생략함 생략시 http는 80, https는 443포트

#### path
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* 리소스 경로(path), 계층적 구조
* 예)
  * /home/file1.jpg
  * /members (회원들의 대한 정보를 보여주는 웹 사이트)
  * /members/100 , /items/iphone12mini...


#### query
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
* key=value 형태
* ?로 시작하고 & 추가 기능 ?keyA=valueA&keyB=valueB
* query parameter, query String 등으로 불림, 웹 서버에 제공하는 파라미터, 문자 형태.

`query String으로 불리는 이유는 어떤 형태도 문자로 넘어감 숫자를 써도 문자형 으로 넘어간다.`


#### fragment
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-introducing-spring-boot

* fragment
* html 내부 북마크 등에 사용
* 서버에 전송하는 정보 아님