# Describe - Context - It 구조

BDD(Behaviour Driven Development)에서 테스트 코드를 작성하는 구조로 구조에 대한 설명은 [BETTER SPECS](https://www.betterspecs.org/#describe)에 자세히 설명이 되어있다.  
키워드를 간단히 요약해보면

- Describe: 테스트할 대상을 명시
- Context: ~하면, ~할 때 같은 상황이나 조건을 명시
- It: ~한다와 같은 대상이 해야 할 행동을 명시

내가 생각한 구조의 가장 큰 장점은 테스트를 계층적으로 작성하여 수행 결과를 파악하기 좋다는 점이다. 수행 결과가 보기 좋아서 자신이 놓친 경우를 찾기 쉽다.

# Junit5의 @Nested

Junit5부터 `@Nested`를 사용하여 계층적으로 테스트 코드를 작성할 수 있다. 우선 `@Nested`를 사용하지 않고 간단한 계산기 테스트 코드를 작성하였다.

## Calculator 클래스

좋은 계산기 코드는 아니지만 간단히 테스트를 위해 작성한 계산기 코드이니 넘어가도록 하자.  
mul()의 경우는 두 수의 곱이 int의 범위를 초과하면, div()의 경우는 0으로 나누면 RuntimeException을 던지도록 하였다.

```java
public class Calculator {
    public double sum(int num1, int num2) {
        return num1 + num2;
    }

    public double sub(double num1, double num2) {
        return num1 - num2;
    }

    public double mul(int num1, int num2) {
        long r = (long) num1 * (long) num2;
        if ((int) r != r) {
            throw new RuntimeException("int의 범위를 벗어남");
        }
        return (int) r;
    }

    public double div(int num1, int num2) {
        if (num2 == 0) {
            throw new RuntimeException("0으로 나눌수 없음");
        }
        return num1 / num2;
    }
}
```

## CalculatorTest 클래스

```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

class CalculatorTest {
    Calculator cal = new Calculator();
    final int NUM1 = 10;
    final int NUM2 = 2;

    @Test
    @DisplayName("sum메소드")
    void sum() {
        assertEquals(cal.sum(NUM1, NUM2), 12);
    }

    @Test
    @DisplayName("sub메소드")
    void sub() {
        assertEquals(cal.sub(NUM1, NUM2), 8);
    }

    @Test
    @DisplayName("mul메소드 범위 정상")
    void mul() {
        assertEquals(cal.mul(NUM1, NUM2), 20);
    }

    @Test
    @DisplayName("mul메소드 결과가 int범위 초과")
    void mul_over() {
        assertThrows(RuntimeException.class, () -> cal.mul(Integer.MAX_VALUE, 4));
    }

    @Test
    @DisplayName("div메소드 0이 아닌수로 나누기")
    void div() {
        assertEquals(cal.div(NUM1, NUM2), 5);
    }

    @Test
    @DisplayName("div메소드 0으로 나누기")
    void div_zero() {
        assertThrows(RuntimeException.class, () -> cal.div(NUM1, 0));
    }
}
```

실행 결과  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/junit/non%20hierarchical%20test.png?raw=true" width="80%">

mul()과 div()메소드는 예외상황에 대한 테스트가 추가되었다.  
실행결과가 한눈에 들어오지 않고 같은 메서드의 테스트 결과가 따로 있어 보기 불편하다. mul()과 div()를 @Nested를 사용하여 변경해보자.

# 계층구조로 변경하기

## CalculatorHierarchicalTest 클래스

테스트 결과가 계층적인 구조로 되었고 테스트가 한 줄로 읽었을 때 하나의 자연스러운 문장이 되어 이해하기 쉽다.

```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

class CalculatorHierarchicalTest {
    Calculator cal = new Calculator();
    final int NUM1 = 10;
    final int NUM2 = 2;

    @Test
    @DisplayName("sum메소드")
    void sum() {
        assertEquals(cal.sum(NUM1, NUM2), 12);
    }

    @Test
    @DisplayName("sub메소드")
    void sub() {
        assertEquals(cal.sub(NUM1, NUM2), 8);
    }

    @Nested
    @DisplayName("mul 메소드는")
    class Describe_mul{
        @Nested
        @DisplayName("두 수의 곱이 int범위를 넘지 않으면")
        class Context_does_not_exceed_int_range{
            @Test
            @DisplayName("두수의 곱을 반환한다.")
            void it_return_int(){
                assertEquals(cal.mul(NUM1, NUM2), 20);
            }
        }

        @Nested
        @DisplayName("두 수의 곱이 int범위를 넘으면")
        class Context_exceed_int_range{
            @Test
            @DisplayName("RuntimeException을 던진다.")
            void it_return_exception(){
                assertThrows(RuntimeException.class, () -> cal.mul(Integer.MAX_VALUE, 4));
            }
        }
    }

    @Nested
    @DisplayName("div 메소드는")
    class Describe_div{
        @Nested
        @DisplayName("0이 아닌수로 나누면")
        class Context_not_divided_zero{
            @Test
            @DisplayName("나눈 결과값을 반환한다.")
            void it_return_int(){
                assertEquals(cal.div(NUM1, NUM2), 5);
            }
        }

        @Nested
        @DisplayName("0으로 나누면")
        class Context_divided_zero {
            @Test
            @DisplayName("RuntimeException을 던진다.")
            void it_return_exception() {
                assertThrows(RuntimeException.class, () -> cal.div(NUM1, 0));
            }
        }
    }
}
```

실행 결과  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/junit/hierarchical%20test.png?raw=true" width="80%">

# 참고 문서

[Junit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)  
[BETTER SPECS](https://www.betterspecs.org/#describe)
