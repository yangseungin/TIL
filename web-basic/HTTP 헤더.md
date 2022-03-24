# HTTP 헤더 - 일반 헤더
```
header-field = field-name":" OWS field-value OWS(OWS: 띄어쓰기 허용)
```
- field-name은 대소문자 구분 없음
- HTTP 전송에 필요한 모든 부가정보

## 표현
- Content-Type: 표현 데이터의 형식에 대한 정보,미디어 타입이나 문자 인코딩(application/json, charset=utf-8...)
- Content-Encoding: 표현 데이터를 압축하기위해 사용(gzip, deflate, identity)
- Content-Language: 표현 데이터의 자연 언어(ko, en)
- Content-Length: 표현 데이터의 길이, 바이트

## 협상
클라이언트가 선호하는 표현 요청으로 요청을 보낼 경우에만 사용한다.
- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 언어

### 우선순위
선호하는 언어의 우선순위를 지정할 수 있다.
- 우선순위는 0 ~ 1 사이의 값이며 클수록 우선순위가 높다
- 생략하면 1

ex)
```
GET /evnet
Accept-Language: ko-KR;en;q=0.7
```
한국어를 가장 선호하며 그다음 순위로 en을 선호한다. 서버가 둘다 지원하지 않는경우 서버의 기본지원 언어로 전달한다.

```
GET /evnet
Accept: text/*, text/plain, text/plain;format=flowed, */*
```
text/plain;format=flowed 다음 text/plain, text/*, */* 순  
구체적으로 명시한것이 가장 우선이다.  
# 참고문서
https://www.inflearn.com/course/http-웹-네트워크/