# HTTP 상태코드
클라이언트의 요청의 처리상태를 응답 메세지에서 알려줌
- 1xx(informational): 요청이 수신돼 처리 중.
- 2xx(successful): 정상 처리됨
- 3xx(redirection): 요청을 완료하려면 추가적인 처리가 필요함
- 4xx(client error): 클라이언트 오류나 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx(server error): 서버 오류. 서버가 정상 요청을 처리하지 못함

# 2xx - successful
클라이언트의 요청을 성공적으로 처리함  
200대에서 자주 볼 수 있는 응답코드  
- 200 ok: 요청 성공
- 201 Created: 요청 성공으로 새로운 리소스 생성
- 202 Accepted: 요청은 접수되었으나 처리가 완료되지 않음
- 204 No Content: 요청을 성공적으로 처리하였으나 보낼 응답값이 없음.

# 3xx - redirection
요청을 완료하기 위해 클라이언트의 추가 조치가 필요함  
3xx 응답 결과에 Location 헤더가 있으면 Location 위치로 이동함  
300대에서 볼 수 있는 응답코드  
- 300 Multiple Choices
  - 거의 사용하지 않음
- 301 Moved Permanently
  -  리소스의 URI 영구 이동
  -  리다이렉트시 요청 메서드가 GET으로 변경
  -  본문이 제거될 수 있음
- 302 Found
  - 리다이렉트시 요청메서드가 GET으로 변함
  - 본문이 제거될 수 있음
- 303 See Other
  - 302와 기능은 같음
  - 리다이렉트시 요청 메서드가 GET으로 변경
- 304 Not Modified
  - 캐시를 목적으로 사용
  - 클라이언트에게 리소스가 수정되지 않았음을 알려줘서 저장된 캐시를 사용하도록 함
- 307 Temporary Redirect
    - 302와 기능은 같음
    - 리다이렉트시 요청 메서드와 본문 유지
- 308 Permanent Redirect
  - 301과 거의 동일하나 본문이 유지됨 

## 리다이렉션 종류
- 영구 리다이렉션: 특정 리소스의 URI가 영구적으로 이동, 검색엔진에서도 변경 인지
  - 301, 308
- 일시 리다이렉션: 리소스의 URI가 일시적인 변경, 검색엔진에서 URL을 변경하면 안됨
  - 302, 303, 307
- 특수 리다이렉션: 결과 대신 캐시 사용
  - 304

# 4xx - client error
클라이언트의 잘못된 요청  
300대에서 볼 수 있는 응답코드  
- 400 Bad Request
  - 클라이언트가 잘못된 요청을 보냄
- 401 Unauthorized
  - 인증이 필요한 요청임을 알려줌
- 403 Forbidden
  - 서버가 요청을 이해했지만 승인을 거부함
  - 주로 인증은 되었지만 인가가 되지 않은 경우
- 404 Not Found
  - 요청 리소스를 찾을 수 없음

# 5xx - server error
서버에 문제가 있어서 오류 발생  

- 500 Internal Server Error
  - 서버 내부 문제로 발생
- 503 Service Unavailable
  - 서비스 이용불가
  - 일시적인 과부하나 예정된 작업으로 처리할 수 없음


# 참고문서
https://www.inflearn.com/course/http-웹-네트워크/