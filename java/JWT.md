# JWT(Json Web Token)이란?

JWT는 인증에 필요한 정보들을 암호화시킨 문자열로 [RFC7519](https://tools.ietf.org/html/rfc7519) 표준입니다.  
사용자는 JSON 객체를 HTTP Authorization 헤더에 실어 서버로 보내서 헤더에 포함된 JWT 정보를 통해 인증을 할 수 있습니다.  
해싱 알고리즘으로는 HMAC SHA256, RSA등이 사용된다.

# JWT 토큰의 구성

JWT 토큰은 크게 세 부분으로 나뉘는데 Header, Payload, Signature로 불리며 문자열에서 온점(.)으로 구분됩니다.

```
xxxxxx.yyyyy.zzzzz
```

## 헤더(Header)

Header는 해싱 알고리즘과 토큰의 타입으로 구성됩니다.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

## 정보(Payload)

Payload는 토큰에 담을 정보를 지닙니다. 여기서 정보를 클레임(Claim)이라고 부르고, name과 value쌍으로 이루어져 있으며 토큰에는 여러 개의 클레임을 넣을 수 있습니다.  
클레임은 크게 세 가지로 분류됩니다.

- Registered claims
  - 토큰에 대한 정보들을 담기 위한 사용
  - 토큰 발급자, 대상자, 만료기간 등등의 정보를 가지고 있음
  ```json
  {
    "iss": "yang",
    "exp": "1485270000000"
  }
  ```
- Public claims
  - 충돌 방지를 위해 클레임의 이름을 URI형식으로 함.
  ```json
  {
    "https://giantdwarf.tistory.com/": true
  }
  ```
- Private claims
  - 서버와 클라이언트 간에 협의하에 사용되는 클레임으로 정보를 공유하기 위해 사용.
  ```json
  {
    "username": "Seungin Yang",
    "admin": true
  }
  ```

전체 payload

```json
{
  "iss": "yang",
  "exp": "1485270000000",
  "https://giantdwarf.tistory.com/": true,
  "username": "Seungin Yang",
  "admin": true
}
```

## 서명(Signature)

서명은 JWT토큰의 마지막 문자열이며 인코딩한 헤더와 정보를 합치고 비밀키로 해쉬하여 생성한 후 base64 형태로 인코딩하여 생성합니다.
예를 들어 HMAC SHA256 알고리즘을 사용하려는 경우 서명은 다음과 같은 방식으로 생성됩니다.

```
//슈도코드
HMACSHA256(
base64UrlEncode(header) + "." +
base64UrlEncode(payload),
secret)
```

이렇게 만든 만든 해쉬를, BASE64로 인코딩하면 토큰이 완성됩니다.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ5YW5nIiwiZXhwIjoiMTQ4NTI3MDAwMDAwMCIsImh0dHBzOi8vZ2lhbnRkd2FyZi50aXN0b3J5LmNvbS8iOnRydWUsInVzZXJuYW1lIjoiU2V1bmdpbiBZYW5nIiwiYWRtaW4iOnRydWV9.Sj1vI3ITY5jQvAchCB-rx_7fh_EcManxmLrZzMSmXQw
```

이 값을 [JWT디버거](https://jwt.io/#debugger-io)에 encoded에 넣어보면 서명이 되는것 까지 확인할 수 있습니다.
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/jwt/jwt%20encode.png?raw=true" width="80%">

JWT를 쉽게 사용하기위한 다양한 라이브러리들이 존재하는데 [jwt.io](https://jwt.io/#libraries-io)에서 확인할 수 있습니다.

# 참고 문서

https://jwt.io/
