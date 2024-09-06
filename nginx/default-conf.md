## Nginx conf 설정

Nginx에서 리다이렉션 설정을 할 때 설정부분에 대해 정리 헀다.

```
return 301 https://$host$request_uri;
```

`return 301:`  
 영구적인 리다이렉션. 클라이언트에서 요청한 URL이 영구적으로 다른 URL로 이동되었음을 알리는 상태 코드입니다.  

<br>

`https://$host:`  
  $host는  클라이언트가 요청은 호스트명을 나타낸다.  
  즉, 사용자가 입력한 도메인 예) (www.example.com 또는 example.com)  
  를 입력했다고 가정했을 시, 이 도메인명으로 그대로 리다이렉션 한다.


  <br>

`$request_uri:`  
 요청한 경로와 쿼리 문자열을 그대로 사용하여 리다이렉트 한다.  
 예를들어 /about?user=kim 같은 요청 경로가 있을 경우 그대로 사용한다.


<br>
 
 #### 전체 해석:
 이 설정은 클라이언트가 요청한 호트스명과 경로를 그대로 유지하면서 HTTPS로 리다이렉션 한다.  
 즉, 호스트명이 바뀌지않고 단지 HTTPS 프로토콜을 적용하는 역할이다.

 `결과적으로 HTTP로 오는 모든 요청을 HTTPS로 변경하는데 사용한다.`


<br>

#### 예:

클라이언트 요청:
```
http://example.com/page?name=test
```

리다이렉션:
```
https://example.com/page?name=test
```

