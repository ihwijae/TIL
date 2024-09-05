# HTTP 표현 헤더

#### 표현 헤더에 대해 정의 하자면, HTTP Body 부분을 설명하기 위한 헤더들 이다.

<br>

## 표현
- Cotent-Type: 표현 데이터의 형식
- Content-Encoding: 표현 데이터의 압축 방식
- Content-Language: 표현 데이터의 자연 언어
- Content-Length: 표현 데이터의 길이 (이 헤더는 명확하게 구분하자면 페이로드 헤더로 구분하는게 맞다.)

`위의 표현 헤더는 공통 헤더이므로 요청, 응답 헤더에 모두 사용한다.`

<br>

>페이로드 헤더(payload header) 는 하나 이상의 메시지에서 원래 리소스 표현의 안전한 전송 및 재구성과 관련된 페이로드 정보를 설명하는 HTTP 헤더입니다.  
>페이로드 헤더에는 메시지 페이로드의 길이, 이 페이로드에 전달되는 리소스 부분(다중 부분 메시지의 경우), 전송에 적용되는 인코딩, 메시지 무결성 검사 등과 같은 정보가 포함됩니다.  
>페이로드 헤더는 HTTP 요청 및 응답 메시지 모두(즉, 페이로드 데이터를 전달하는 모든 메시지)에 존재할 수 있습니다.


<br>

## Content-Type
#### 표현 데이터 형식 설명
- 미디어 타입, 문자 인코딩
- 예)
  - text/html;charset=utf-8
  - application/json (json은 기본이 utf-8 이라서 charset=utf-8 생략 가능)
  - image/png

<br>

## Content-Encodion
#### 표현 데이터 인코딩

- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서(요청,응답 모두 해당) 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
- 예)
  - gzip
  - deflate
  - identity

<br>

## Content-Language
#### 표현 데이터 자연언어
- 표현 데이터의 자연 언어를 표현 (영어, 한국어 ..)
- 예)
  - ko
  - en
  - en-US

<br>


## Content-Length
#### 표현 데이터 길이
- 바이트 단위
- Transfer-Encoding(전송 코딩)을 사용하면 Content-Length 필드를 사용하면 안된다.