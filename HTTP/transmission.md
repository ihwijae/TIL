## 전송 방식

- Transfer-Encoding
- Range, Content-Range

#### 방식 설명
- 단순 전송
- 압축 전송
- 분할 전송
- 범위 전송

<br>

## 단순 전송
#### Content-Length

클라이언트 요청
```
GET /event
```

서버 응답
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423

<html>
    <body>....</body>
</html>
```

단순하게 메시지 본문의 길이를 알 수 있을때 사용한다  
Content-Length: 3423 이렇게 지정  
단순하게 요청하고  응답을 한번에 다 받는 것.

`참고로 메시지 본문의 양이 적으면 단순하게 전송하면 되는데 양이 많아지면 분할 전송을 사용하면 좋다.`


<br>

## 압축 전송
#### Content-Encoding


클라이언트 요청
```
GET /event
```


서버 응답
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip
Content-Length: 521

edasdsadsad;12131dnasjd12412hjas
```

서버에서 압축해서 보내는 전송 방식 이다.  
실제로 용량이 절반 이상 줄어든다고 한다.  
Content-Encoding: 필드에 형식을 넣는다.  



<br>

## 분할 전송
#### Transfer-Encoding

클라이언트 요청
```
GET /event
```


서버 응답
```
HTTP/1.1 200 OK
Contetn-Type: text/plain
Transfer-Encoding: chunked


5
Hello
5
World
0
\r\n
```

`chunked: 덩어리 즉, 덩어리로 쪼개서 보낸다`  
네트워크가  
1. 5byte를 먼저 보내고 내용은 Hello
2. 5byte를 보내고 내용은 World
3. 0 \r\n은 끝났다는 뜻.

서버는 먼저 Hello를 보내고, 그다음 서버에서 World가 만들어지면 다시 보내고  ..

`Content-Length 필드를 넣으면 안된다. 처음에 예상이 안된다. 분할해서 보내기 때문에 청크마다 byte길이가 있기 때문이다.`


<br>

## 범위 전송
#### Range, Content-Range

클라이언트 요청
```
GET /event
Range: bytes=1001-2000
```

서버 응답
```
HTTP/1.1 200 OK
Contetn-Type: text/plain
Content-Range: bytes 1001-2000 / 2000

qsjdnajsdnajd241254edsfa3245jb23j4h12j
```

예를들어 클라이언트에서 이미지를 받는다고 가정해본다.  
절반정도 받았는데 중간에 에러가 나서 끊겼다.  
처음부터 다시 요청하면 용량이 아깝기 때문에  
받은부분은 제외하고 범위를 지정해서 전송할 수 있다.  

`해당 기능으로 다운로드와 같은 상황에서 일지정지 & 이어받기 가 가능하다`

파일을 다운받는 과정  
 1) 임시저장소에 저장한 이후 다운로드가 완료되면
2) 최종 디렉토리로 옮기는 과정을 거칩니다 (사용자가 다운받기로 지정한 경로 혹은 기본경로)



> 기본적으로 분할전송과 범위전송은 같은 메커니즘이다.  
> 100이라는 파일을 예를들어 1-10, 11-20, 21-30 ....91-100 까지 나누어 보내는 것이 분할전송.    
> 결국 범위전송을 10번 하는 것과 같다.  
> 만약 분할전송 중 두 번째 전송(11-20 의 내용을 담은 전송) 이 제대로 처리 되지 않았다면  
> 클라이언트는 11-20까지의 내용을 다시 서버에게 요청하게 되고, 서버는 해당 범위만큼만 범위전송 한다.

