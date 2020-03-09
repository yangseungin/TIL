# 제어의 역전(Inversion of Control)
IoC는 간단히 말해 프로그램의 제어 흐름을 개발자가 아닌 프레임워크가 주도하는 것이다.  
객체의 생성, 생명주기의 관리를 컨테이너가 맡아서 하게되어 제어권이 컨테이너로 넘어갔다고 하여 IoC(Inversion of Control)이라고 한다.  
제어권이 컨테이너로 넘어감으로써 DI(의존성주입), AOP(관점지향 프로그래밍)등이 가능하게 되었다.  

# 의존성 주입(Dependency Injection)
스프링은 기본적으로 DI기반으로 동작하기 때문에 DI에 대한 이해가 필수적이다.  
DI는 객체 사이의 의존성을 자신이 아닌 외부로부터 전달받는 구현방식으로 생성자 또는 세터(Setter)를 통해 전달받는 것을 말한다.
객체를 주입받지 않고 직접 생성하는 방법의 단점을 알아보고 이를 의존성 주입이 왜 필요하게 되었는지 아래 예제를 통해 알아보자.  

### 의존객체를 직접 생성하여 사용
~~~ java
public class Customer {
    private Vehicle vehicle;

    public Customer() {
        this.vehicle = new Car();   //의존 객체를 직접 생성
    }
    public void showVehicle(){
        vehicle.show();
    }
}

interface Vehicle {
    public void show();
}

class Car implements Vehicle{
    @Override
    public void show() {
        System.out.println("Car");
    }
}


class Train implements Vehicle{
    @Override
    public void show() {
        System.out.println("Train");
    }
}
~~~
위의 코드에서 고객(Customer)는 탈것(Vehicle)객체를 직접 생성하고 있다. 이 상황에서 요구의 변화로 Car가 아닌 Train 클래스를 사용해야하는 상황이 발생하였다. 이 경우 Car 객체를 직접 생성하여 의존하고있는 클래스가 하나라면 다행이나 여러곳일 경우 반복적인 작업으로 수정해야되며 이는 개발 생산성의 저하로 이어진다.

### 의존객체를 전달받는 방법
의존객체를 전달받는 방법에는 생성자를 이용하는 방법과 Setter를 이용하는 방법이 있다
~~~java
public class Customer {
    private Vehicle vehicle;

    public Customer(Vehicle vehicle) {
        this.vehicle = vehicle; // 생성자로 전달받은 객체를 할당
    }
}

public class Customer {
    private Vehicle vehicle;

    public void setVehicle(Vehicle vehicle) {
        this.vehicle = vehicle; // set 메서드를 통해 의존 객체 전달
    }
}
~~~
new를 통해 직접 객체를 생성하던 방식에서 외부로 부터 전달받는 방식으로 변경하였다.  
이 경우 요구사항이 바뀌어도 Customer 클래스의 코드는 수정할 필요없이 변경되는 부분은 제한된다.  
생성자 방식의 장점은 객체의 생성시점에 의존하는 객체를 모두 전달받을 수 있어서 전달받은 객체가 정상인지 확인하는 코드를 추가하여, 생성시점 이후에는 그 객체가 사용가능 상태임을 보장할 수 있다.  
set 메서드를 통한 방식의 장점은 메서드의 이름으로 어떤 객체를 의존하는지 이름으로 알 수 있다.  

# 스프링 IoC 컨테이너
스프링에서는 컨테이너(Container)라는 곳에서 스프링 IoC가 관리하는 객체인 bean 들을 생성과 관리를 담당하며 의존성을 관리한다.
스프링 IoC 컨테이너에 등록되는 빈들은 기본적으로 싱글톤 scope의 빈으로 등록된다.

## 스프링 DI 설정
스프링 IoC 컨테이너는 빈 설정은 XML, 자바 코드, 그루비 코드의 세가지 방법으로 가능하다.
여기서 빈(bean)은 스프링 IoC가 관리하는 객체라고 생각 하면 된다.

### 1. XML을 이용한 설정
~~~ java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="boookService"
          class="com.giantdwarf.book.BookService">
        <property name="bookRepository" ref="bookRepository"/>
    </bean>

    <bean id="bookRepository"
          class="com.giantdwarf.book.BookRepository">

    </bean>

</beans>
~~~
\<bean\> 태그: 생성할 객체를 지정할 때 사용하며 id는 빈의 식별자, class는 생성할 객체의 클래스 이름이다.  
\<constructor-arg\> 태그: 생성자 방식의 태그
\<property\> 태그: 프로퍼티 방식을 사용하는 경우 사용하며 name은 프로퍼티의 이름, ref는 다른 빈의 식별자를 입력한다.  

xml 설정파일을 만들었다면 GenericXmlApplicationContext 클래스를 사용하여 스프링 컨테이너를 생성하여 컨테이너에 bean 객체가 생성됬는지 확인 할 수 있다.
~~~ java
public class Sample {
    public static void main(String[] args) {
        GenericXmlApplicationContext ctx = new GenericXmlApplicationContext("classpath:application.xml");
        BookRepository bookRepository = (BookRepository) ctx.getBean("bookRepository");
        System.out.println((bookRepository!=null)); //true
        
    }
}
~~~
위의 방식의 단점은 일일히 하나씩 빈을 등록해야해서 번거롭다.  
그래서 등장한 방식이 context의 component-scan이다. 이 방법은 설정한 패키지부터 빈을 스캔하여 빈으로 등록하는 것이다.
~~~java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.giantdwarf.book" />
</beans>
~~~
그리고 스캔할 클래스에 @Component 애노테이션을 적용하고 <context:component-scan> 태그를 이용하여 검색할 패키지를 지정하면 된다.  
이떄 @Autowired와 같은 애노테이션을 이용하여 프로퍼티에 의존객체를 전달할 수 있다.
~~~java
@Component
public class BookService {

    @Autowired
    BookRepository bookRepository;

    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }
}
~~~

### 2. 자바 코드을 이용한 설정
자바 코드를 사용하면 좀더 직관적으로 설정할 수 있다.
자바코드를 스프링 설정 정보로 사용하기위해서는 @Configuration과 @Bean 애노테이션을 사용하면된다.
~~~java
@Configuration
public class ApplicaionConfig {

    @Bean
    public BookRepository bookRepository(){
        return new BookRepository();
    }

    @Bean
    public BookService bookService(){
        BookService bookService = new BookService();
        bookService.setBookRepository(bookRepository());
        return bookService;
    }
}
~~~
자바 설정 클래스를 이용해 컨테이너를 생성하기 위해서는 AnnotationConfigApplicationContext 클래스를 사용한다.
~~~java
public class Sample {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext  ctx = new AnnotationConfigApplicationContext(ApplicaionConfig.class);
        BookRepository bookRepository = (BookRepository) ctx.getBean("bookRepository");
        System.out.println((bookRepository!=null)); //true

    }
}
~~~
자바 설정 파일에서 역시 @ComponentScan 애노테이션을 통해 컴포넌트 스캔을 지원한다.
~~~java
@Configuration
@ComponentScan(basePackageClasses = Sample.class)
public class ApplicaionConfig {
}
~~~