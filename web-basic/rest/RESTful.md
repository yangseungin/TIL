API를 개발하다 보면 혹은 많은 회사의 JD의 자격 요건을 보면 이러한 문장을 흔히들 볼 수 있다.

> RESTful API 설계 및 개발 경험

누구나 흔하게 REST라는 말을 쓰지만 잘 모르겠는 주제인데 최근에 플람님이 아래와 같은 질문을 주셨다.   

> RESTful 하게 개발하려 하신 것 같은데 호출 URL 들이 동사로 되어있는 것 같은데 어떤 의도가 있으신가요?

그 질문을 받고 순간 아차 싶었고 순간 당황해서 `RESTful하게 개발하려 했는데 그 부분은 놓친 것 같다`라고 답변하였지만 명확하게 정리하지 못한 것 같아서 정리를 해보려 한다.

# RESTful API  
REST는 `REpresentational statue Transfer`의 약자로 웹의 장점을 최대한 잘 활용할 수 있는 제약조건의 집합이다.  
REST 아키텍쳐 스타일은 아래의 6가지 제약 조건 중 일부 혹은 전체를 준수하면 RESTful이라고 한다.  

## client-server  
클라이언트와 서버는 HTTP 프로토콜을 사용해서 통신한다. 클라이언트가 서버로 요청을 보내면 서버는 요청에 따라 적절한 응답을 클라이언트로 회신한다.  
클라이언트와 서버가 서로 독립적이어서 별도로 진화할 수 있다.

## stateless  
클라이언트의 애플리케이션 상태에 대한 정보를 서버에서 관리하지 않는다. 

## cache  
HTTP 프로토콜을 사용하기 때문에 응답을 캐시 할 수 있다. 

## uniform interface  
인터페이스가 일관되어야 한다. RESTful API라면 HTTP 프로토콜을 따르는 모든 클라이언트에서 사용할 수 있다.  

### uniform interface의 제약조건  
- identification of resources: 리소스가 URI로 식별되면 된다.
- manipulation of resources through representations: representation 전송을 통해 리소스를 조작해야 된다.
- self-descriptive message: 메세지는 스스로를 설명해야 한다.
- hypermedia as the engine of application state(HATEOAS): 애플리케이션의 상태는 Hyperlink를 통해 전이되어야 한다.

오늘날 rest api라고 하는 것들의 대부분이 self-descriptive message와 HATEOAS를 지키지 못하고 있다.

## layered system  
시스템을 몇 개의 계층으로 분리할 수 있다. 클라이언트는 서버에 직접 연결되어 있는지 중간에 연결되어 있는지를 알 수 없다.


# 참고문서
[DEVIEW - 그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)  
[위키피디아 - Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer#Layered_system)  