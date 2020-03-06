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

    public void showVehicle(){
        vehicle.show();
    }
}
~~~
new를 통해 직접 객체를 생성하던 방식에서 외부로 부터 전달받는 방식으로 변경하였다.  
이 경우 요구사항이 바뀌어도 Customer 클래스의 코드는 수정할 필요없이 변경되는 부분은 제한된다.

### 스프링 IoC 컨테이너
스프링에서는 컨테이너(Container)라는 곳에서 스프링 IoC가 관리하는 객체인 bean 들을 생성과 관리를 담당하며 의존성을 관리한다.
스프링 IoC 컨테이너에 등록되는 빈들은 기본적으로 싱글톤 scope의 빈으로 등록된다.