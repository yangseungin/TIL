# 스프링부트의 오류처리(Error Handling)
기본적으로 스프링 부트는 모든 에러를 처리하는 /error 매핑을 제공하며 서블릿 컨테이너에 전역 오류페이지로 등록한다.  
머신 클라이언트의 경우는 오류, http 상태 exception메세지를 json으로 응답하며 브라우저 클라이언트에서는 동일한 데이터를 html 포맷으로 렌더링하는 "whitelabel" 에러 뷰를 보여준다.
- Browser Client  
<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/browser%20client.png?raw=true" width="70%">

- Machine Client  
<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/machine%20client.png?raw=true" width="70%">


# BasicErrorController
스프링 부트는 BasicErrorController에서 에러핸들링을 하고있다. 
생성자와 stacktrace, errors, message속성에대한 결정을 하는 메서드들은 생략하였다.

~~~ java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {

	@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
		HttpStatus status = getStatus(request);
		Map<String, Object> model = Collections
				.unmodifiableMap(getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.TEXT_HTML)));
		response.setStatus(status.value());
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
	}

	@RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		HttpStatus status = getStatus(request);
		if (status == HttpStatus.NO_CONTENT) {
			return new ResponseEntity<>(status);
		}
		Map<String, Object> body = getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.ALL));
		return new ResponseEntity<>(body, status);
	}
}
~~~
하나씩 확인해보자
~~~ java
//                        1         :       2    :   3      
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {
    ...
}
~~~
 property에 server.error.path가 있으면 그 값을 없을경우 error.path를 사용하고 둘다 없는 경우 /error를 맵핑한다.

~~~ java
@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
        ...
    }
~~~
Accept Header에 text/html가 포함된 경우 errorHtml()에서 응답을 처리한다.
~~~ java
@RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		...
	}
~~~
그렇지 않은 경우에는 error()에서 ResponseEntity로 리턴한다(json)
 
 # Custom error page
 응답의 상태코드에 따라 에러페이지를 다르게 보여줄 수있다.  
 기본 경로는 src/main/resources의 static 혹은 templates에 error 디렉토리를 만들어주고 파일명을 정확한 상태코드나 시리즈 마스크로 하면 된다.  
 여기서는 템플릿 엔진으로 Thymeleaf를 사용하였다.
 ~~~
 src/
 +- main/
     +- java/
     |   + <source code>
     +- resources/
         +- templates/
             +- error/
             |   +- 404.html
             |   +- 5xx.html
             +- <other public assets>
 ~~~

404.html
~~~ html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>404 error!</h1>
<p th:text="${'timestamp: ' + timestamp}"></p>
<p th:text="${'path: ' + path}"></p>
<p th:text="${'status: ' + status}"></p>
<p th:text="${'timestamp: ' + timestamp}"></p>
<p th:text="${'error: ' + error}"></p>

</body>
</html>
~~~
5xx의경우 동일하며 h1태그의 내용만 505로 하였다.
<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/404.png?raw=true" width="50%">
<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/5xx.png?raw=true" width="50%">  
이처럼 응답코드로 view의 이름을 작성하여 커스텀한 에러 페이지를 작성할 수 있다.

# @ExceptionHandler
스프링에서는 @ExceptionHandler를 통해 발생한 예외 처리를 할 수 있다.

Controller에서 예외를 던지도록 추가하였다.
~~~ java
//ErrorData
public class ErrorData {
    private String message;
    private String code;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }
}

//TestException
public class TestException extends RuntimeException {
    public TestException() {
        super();
    }

    public TestException(String message) {
        super(message);
    }
}

//ExceptionController
@RestController
public class ExceptionController {
    @GetMapping("/exception")
    public String exception(){
        throw new TestException("알수없는 에러 발생");
    }
    @ExceptionHandler(TestException.class)
    public ResponseEntity sampleError(TestException e){
        ErrorData error = new ErrorData();
        error.setCode("ERROR_UNKNOWN");
        error.setMessage(e.getMessage());

        return ResponseEntity.status(HttpStatus.OK).body(error);
    }

~~~
이처럼 특정 Controller에서 예외가 발생하는 경우 @ExceptionHandler를 서칭하여 예외를 처리할 수 있다.  

오류가 발생하는 /exception으로 요청을 날리면 다음과 같은 응답을 얻을 수 있다.
~~~
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Sun, 05 Jul 2020 11:53:00 GMT
{"message":"알수없는 에러 발생","code":"ERROR_UNKNOWN"}%  
~~~

# 참고문서
[spring reference: error handling](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-error-handling)