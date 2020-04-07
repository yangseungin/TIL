# AOP란?  
Aspect Oriented Programming을 줄여서 aop 라고 하며 문제를 바라보는 관점을 기준으로 프로그래밍 하는 기법으로 핵심적인 관심사항과 공통적인 관심사항(cross-cutting concern)을 기준으로 모듈화 하는 프로그래밍 기법이다.  

## AOP 용어 정리
Aspect: 여러 객체에 공통으로 적용하는 기능  
Target: Aspect를 적용하는 대상
Advice: 언제 공통 핵심기능을 핵심 로직에 적용할지를 정의 ex)메서드 호출 전  
Joinpoint: Advice를 적용가능한 지점. 메서드 호출, 생성자 호출시점, 필드 값 변경 등  
Pointcut: Joinpoint의 부분집합으로 실제로 Advice가 적용되는 Joinpoint  
Weaving: 공통 코드를 핵심로직 코드에 삽입하는 것

## Weaving 방법
Advice를 Weaving하는 방법은 총 3가지이다.
- 컴파일 타임
- 클래스 로드 타임
- 런타임

컴파일 시에 적용하는 방법은 AspectJ에서 사용하는 방법으로 자바 코드를 class파일로 컴파일할 때 바이트 코드를 조작하여 AOP가 적용된 클래스 파일을 생성한다.  
클래스 로드 시에 적용하는 방법은 JVM이 클래스 파일을 로딩하는 시점에 클래스 정보를 변경하여 사용한다.(원본 클래스파일은 변경되지 않으며 AspectJ는 클래스 로드 방식도 지원한다.)  
런타임 시 적용하는 방법은 스프링AOP가 사용하는 방법으로 프록시를 이용하여 AOP를 적용한다.  
핵심 로직을 구현한 객체에 직접 접근하지 않고 프록시를 생성하고 이 프록시를 통해서 핵심로직을 구현한 객체에 접근한다.

# 스프링 AOP
스프링은 자체적으로 프록시기반의 AOP이다. 따라서 Joinpoint만을 지원한다.  
스프링 빈에만 AOP를 적용할 수 있으며 AOP의 모든 기능을 제공하는것이 목적이 아니라 엔터프라이즈 애플리케이션에서 필요한 기능을 제공하는 것을 목적으로 하고 있다.

### 프록시 기반의 AOP 구현
스프링은 타겟이 되는 객체에 대한 프록시를 만들어 제공하며, 대상 객체에 직접 접근하지 않고 프록시를 통해서 접근하게 된다.  
클라이언트는 인터페이스를 통해 필요한 메서드를 호출하게 되고 인터페이스에 정의되지 않은 메서드는 AOP가 적용되지 않는다.  

<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%20%EA%B8%B0%EB%B0%98%EC%9D%98%20%ED%94%84%EB%A1%9D%EC%8B%9C.png?raw=true" width="70%">

위의 프록시 구조로 성능 측정을 하는 예시를 확인해보자.
~~~ java
public interface EventService {
    void createEvent();
    void stopEvent();
    void restartEvent();
}
~~~
~~~ java
@Service
public class TestEventService implements EventService {

    @Override
    public void createEvent() {
        long start = System.currentTimeMillis();
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("create event");
        System.out.println(System.currentTimeMillis() - start);

    }

    @Override
    public void stopEvent() {
        long start = System.currentTimeMillis();
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("stop event");
        System.out.println(System.currentTimeMillis() - start);
    }

    @Override
    public void restartEvent() {
        System.out.println("restart event");
    }

}
~~~
메서드의 실행시간을 측정하는 하고자 할 떄 시간을 측정하여 출력하는 부분들이 공통적인 관심사항이다. 실행시간 측정을 위한 흩어진 관심사들이 기존 코드에 추가되는 문제점이 있는데 이를 프록시 패턴을 사용하여 해결할 수 있다.

~~~ java
@Primary
@Service
public class ProxyEventService implements EventService {

    @Autowired
    TestEventService testEventService;

    @Override
    public void createEvent() {
        long start = System.currentTimeMillis();
        testEventService.createEvent();
        System.out.println(System.currentTimeMillis() - start);
    }

    @Override
    public void stopEvent() {
        long start = System.currentTimeMillis();
        testEventService.stopEvent();
        System.out.println(System.currentTimeMillis() - start);
    }

    @Override
    public void restartEvent() {
        testEventService.restartEvent();

    }
}
~~~
하지만 중복코드가 있는 문제점과 새로 프록시 클래스를 만들어야하는 번거로움 등 여러 문제들이 남아 있다. 프록시를 클래스로 만들어서 사용하였지만 스프링AOP를 통해 동적으로 프록시 객체를 만들어서 사용할 수 있다.  


스프링 AOP를 사용하려면 의존성을 추가해야 한다.  
스프링 부트가 아니라면 spring-aop와 aspectjweaver를 추가해주면 된다.
~~~ java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~

@Aspect 에노테이션을 사용하여 Aspect 클래스를 구현하였고 @Component을 통해 스프링 빈으로 등록하였다.

~~~ java
@Component
@Aspect
public class MeasureAspect {

    @Around("execution(* com.giantdwarf.aop.EventService.*(..))")
    public Object timeMeasure(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        Object proceed = proceedingJoinPoint.proceed();
        System.out.println(System.currentTimeMillis()-start);
        return proceed;
    }
}
~~~
@Around에 execution pointcut 표현식을 사용하여 어디에 적용할지를 정의할 수 있다.(@annotation과 bean으로도 가능하다)

@Around외에도 Aspect 실행 시점을 지정할수 있는 다양한 애노테이션들이 있다.  

- @Before(): 대상 객체의 메서드 실행 이전에 호출
- @AfterReturning(): 대상 객체의 메서드가 정상적으로 실행 된 이후 호출
- @AfterThrowing(): 대상 객체의 메서드가 Exception을 발생시킨 경우 호출
- @After(): 타겟 메서드 실행 이후 호출
- @Around(): 타겟 메서드를 감싸서 타겟 메서드 호출 전,후에 어드바이스 기능 수행
