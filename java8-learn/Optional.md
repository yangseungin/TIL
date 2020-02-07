# Optional
Optional<T>는  T타입의 객체를 감싸는 래퍼 클래스(Wrapper class)여서 optional타입의 객체는 모든 타입의 참조변수를 담을 수 있다.  
최종 연산의 결과를 그냥 반환하지 않고 Optional객체에 담아 반환하여 반환된 결과가 null인지 체크하는 조건문 없이 null값으로 인해 발생하는 예외를 처리할 수 있다.

## Optional 객체 생성
of()나 ofNullable()메서드를 사용하여 Optional객체를 생성할 수 있다.  
참조 변수의 값이 null일 가능ㄴ성이 있으면, of()대신 ofNullable()을 사용하여 생성하는것이 좋습니다.  
( of()메서드는 매개변수의 값이 null이면 NPE를 발생하기 때문)

~~~ java
Optional<String> val = Optional.of("abc");
Optional<String> val2 = Optional.of(null);  //NullPointerException 발생
Optional<String> val3 = Optional.ofNullable(null);  //ok
~~~

