# HTTP 메서드 속성



## HTTP 메서드의 속성
- 안전(Safe Methods)
- 멱등(Idempotent Methods)
- 캐시가능(Cacheable Methods)


### 안전 Safe
- 호출해도 리소스가 변경되지 않는다
- Q: 그래도 계속 호출해서, 로그 같은게 쌓여서 장애가 발생한다면?
- A: 안전은 해당 리소스만 고려한다. 그런 부분 까지는 고려하지 않는다.