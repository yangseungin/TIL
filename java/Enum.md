# Enum

열거형은 JDK1.5부터 추가되었으며 서로 관련 있는 상수를 편하게 선언하기 위해 사용할 수 있다.

## enum 정의하는 방법

열거형의 정의는 괄호 안에 상수의 이름을 나열하면 된다.

```java
enum 열거형이름 {상수명1, 상수명2,...}
```

```java
public enum Fare {
    BUS,
    TRAIN,
    AIRPLANE,
    SHIP
}
```

사용할 때는 `열거형이름.상수명`으로 사용하면 된다.

```java
Fare.BUS
```

열거형에 상수값을 사용자가 지정하여 사용하고 싶을 때는 이름 옆에 소괄호를 사용하여 적어주면 되고 값을 저장할 변수와 생성자를 추가해야 한다.

```java
enum Fare {
    BUS(1200),
    TRAIN(10000),
    AIRPLANE(30000),
    SHIP(50000);

    private final int fare;

    Fare(int fare) {
        this.fare=fare;
    }

    public int getFare() {
        return fare;
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        Fare airplane = Fare.AIRPLANE;
        System.out.println(airplane+"의 요금은 "+airplane.getFare());

    }
}
```

실행 결과

```
AIRPLANE의 요금은 30000
```

# enum이 제공하는 메소드 (values()와 valueOf())

## values() 메소드

values()메소드는 enum에 선언된 상수들을 배열로 반환한다.

```java
public class Test {
    public static void main(String[] args) {
        Fare[] values = Fare.values();
        for (Fare value : values) {
            System.out.println(value);
        }
    }
}
```

실행결과

```
BUS
TRAIN
AIRPLANE
SHIP
```

## valueOf() 메소드

valueOf(String name) 메소드는 name과 일치하는 열거형 상수를 반환한다. name과 일치하는 상수가 없는 경우 예외를 발생한다.

```java
public class Test {
    public static void main(String[] args) {
        Fare fare = Fare.valueOf("BUS");
        System.out.println(fare.getFare());
        Fare fare2 = Fare.valueOf("SPACESHIP");
        System.out.println(fare2.getFare());
    }
}
```

실행 결과

```
1200
Exception in thread "main" java.lang.IllegalArgumentException: No enum constant SPACESHIP
	at java.base/java.lang.Enum.valueOf(Enum.java:264)
	at Fare.valueOf(Fare.java:3)
	at Test.main(Test.java:12)
```

# java.lang.Enum

Enum 클래스는 모든 열거형의 부모 클래스로 열거형을 조작하기 위한 다양한 메소드들을 가지고 있습니다. 위에서 언급한 valueOf 외에도 getDeclaringClass(), name(), ordinal()과 같은 메소드들이 있다.

- getDeclaringClass(): 열거형의 Class 객체 반환
- name(): 열거형 상수의 이름을 문자열로 반환
- ordinal(): 열거형 상수가 정의된 순서를 반환(0부터 시작)

## ordinal메서드 대신 인스턴스 필드를 사용하라

이펙티브 자바에서 ordinal메서드가 열거 타입에 몇 번째 위치인지를 반환하다 보니 상수와 연결된 정수가 필요할 때 ordinal메서드를 이용하고 싶을 수도 있다고 하였다. 근데 이는 상수의 선언순서가 바뀌거나 사용 중인 정수는 추가할 수 없으니 사용하지 말라고 하고 있다.

```java
public class Test {
    public static void main(String[] args) {
        Fare fare = Fare.valueOf("SHIP");
        System.out.println(fare.ordinal());
        System.out.println(fare.name());
        System.out.println(fare.getDeclaringClass());

    }
}
```

```
3
SHIP
class Fare
```

# EnumSet

java.util 패키지에 정의되어있는 클래스로 개인적으로 EnumSet은 거의 사용해본 기억이 없는듯하다.  
EnumSet은 이름에서 알 수 있듯 Set인터페이스를 구현한 것으로 추상클래스여서 new연산자를 통해 객체를 생성할 수 없다.

```java
import java.util.EnumSet;

public class Test {
    public static void main(String[] args) {
        EnumSet<Fare> allVehicle = EnumSet.allOf(Fare.class); //모든 Fare Enum을 가진 set 반환
        for (Fare vehicle : allVehicle) {
            System.out.println(vehicle);
        }
        EnumSet vehicleOnLoad = EnumSet.of(Fare.BUS, Fare.TRAIN); //특정값으로 set 반환
        System.out.println("땅에서 탈것 " + vehicleOnLoad);
        EnumSet notLand = EnumSet.complementOf(vehicleOnLoad);  //특정값을 제외하고 set 반환
        System.out.println("땅에서 탈수없는것 " + notLand);


    }
}
```

실행 결과

```
BUS
TRAIN
AIRPLANE
SHIP
땅에서 탈것 [BUS, TRAIN]
땅에서 탈수없는것 [AIRPLANE, SHIP]
```

# 참고 문서

Java의 정석 - 남궁 성  
Effective Java 3/E - 조슈아 블로크  
http://www.tcpschool.com/java/intro
