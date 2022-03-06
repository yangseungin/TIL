# HTTP 특성
HTTP의 가장큰 특성 두가지로 무상태(statusless)와 비연결성(connectionless)가 있다.

## Statusless
서버가 클라이언트의 이전 상태를 보관하지 않는다는 의미.  
상태를 보관하지 않으므로 클라이언트의 응답에 어떤 서버가 응답을 하더라도 상관이 없다.(요청이 많이 증가해도 서버를 증설하여 쉽게 해결할 수 있다.)

## Connectionless
비 연결성은 클라이언트와 서버가 연결을 맺은 후 요청에 대한 응답을 마치면 맺었던 연결을  끊어버리는 성질

### 비연결성의 장점
- 서버 자원을 효율적으로 사용 가능

### 비연결성의 단점
- 연결이 끊어지고 새로 연결할떄 TCP?IP연결을 새로 맺으므로 3 way-handshake하는데 리소스를 소모한다
- 하나의 요청에서 여러 데이터를 전달하는경우 위의 문제가 더 심해질 수 있다.

현재는 하나의 요청에 여러 데이터를 요구하는경우 해당 연결을 맺은 후 응답할 데이터를 모두 응답하고 연결을 끊는 `HTTP 지속연결(Persistent Connections)`로 이를 해결하였다.

# HTTP 메시지
HTTP 메시지는 서버와 클라이언트 간에 데이터를 교환하는 방식으로 요청메시지와 응답 메시지로 구분된다.

## 메세지 형식
```
HTTP-message = start-line
              *( header-field CRLF )
              CRLF
              [ message-body ]
```

## header-field
HTTP 프로토콜 상에서 통신을 할떄 필요한 패킷에 대한 정보를 담고 있다.(Data, client-ip, accept, content-type, ...)

## message-body
실제 전송할 데이터로 html, 이미지, 영상, JSON 등 byte로 표현할 수 있는 모든 데이터를 담을 수 있다.