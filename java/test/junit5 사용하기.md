# JUnit5란?

JUnit은 자바에서 단위테스트를 제공하는 테스트 프레임워크로 테스트를 작성하고 실행할 수 있다.
junit5는 공식 문서에서는 3가지의 서브 프로젝트의 모듈로 구성되어있다고 한다.

```
JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage
```

- Platform  
   JVM에서 테스트 프레임워크를 시작하기 위한 기초 역활
- Jupiter  
   Junit5에서 테스트를 작성과 확장을 위한 새로운 프로그래밍 모델과 확장 모델의 조합
- Vintage  
   하위 버전(JUnit3, JUnit4)을 위한 테스트엔진제공

junit5은 런타임에는 Java 8이상 이 필요하나 이전 버전의 JDK로 컴파일된 코드는 테스트할 수 있다.

# 테스트 환경 확인

- Ide: IntelliJ Idea Ultimate
- oS: Mac OS Big Sur
- Java8
- Gradle 6.8

## 의존성 추가

사용을 위해서는 의존성을 추가해야 한다. [Maven Repository](https://mvnrepository.com/)에서 의존성을 검색하여 추가할 수 있다. 빌드 도구로는 Gradle을 사용할 것이다.  
프로젝트를 아직 생성하지 않았다고 하면 Gradle을 사용하여 프로젝트를 생성할 수 있다.

```
gradle init
```

이때 테스팅 프레임워크를 선택할 수 있는데 JUnit Jupiter를 선택하면 의존성이 추가된 채로 프로젝트가 생성된다.

이미 프로젝트가 존재한다고 하면 `build.gradle`에 아래 내용들을 추가해야 한다.

- JUnit5를 사용하기 위해 플랫폼 활성화

```
test {
    useJUnitPlatform()
}
```

- JUnit Jupiter 의존성 추가  
  [Maven Repository](https://mvnrepository.com/)에서 `JUnit Jupiter API`와 `TestEngine`을 가져와서 추가하자

```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.2'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
}
```

## 동작확인

간단한 테스트를 작성하여 확인해 볼 수 있다.

```java
class AppTest {
    @Test
    void sumTest() {
        assertEquals(100,Integer.sum(40,60));
    }
}
```

사진변경후 저장  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/junit/%EC%83%98%ED%94%8C%20%ED%85%8C%EC%8A%A4%ED%8A%B8.png?raw=true" width="80%">

# 어노테이션

Jupiter에서는 다양한 어노테이션으로 테스트를 구성하고 프레임워크 확장을 지원한다.  
여기에서는 모든 어노테이션을 다루진 않고 자주 사용하였던 어노테이션들을 정리하였다.

| Annotation         | 설명                                                                                                                                             |
| ------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| @Test              | 테스트 메소드임을 나타냄<br> junit4와 다르게 옵션과 인자가 없음                                                                                  |
| @ParameterizedTest | 하나의 메소드로 여러가지 파라미터에 대해 테스트                                                                                                  |
| @RepeatedTest      | 파라미터만큼 테스트를 반복                                                                                                                       |
| @TestMethodOrder   | 테스트 메서드의 실행순서를 구성할때 사용 <br> JUnit4의 @FixMethodOrder                                                                           |
| @TestInstance      | 테스트 인스턴스 생명주기를 구성하는데 사용                                                                                                       |
| @DisplayName       | 테스트 클래스나 메소드의 이름을 선언                                                                                                             |
| @BeforeEach        | 현재 클래스에서 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`가 적힌 각각의 메소드가 실행되기 전에 실행<br> JUnit4의 @Before    |
| @AfterEach         | 현재 클래스에서 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`가 적힌 각각의 메소드가 실행된 후에 실행<br> JUnit4의 @After       |
| @BeforeAll         | 현재 클래스에서 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`가 적힌 모든 메소드가 실행되기 전에 실행<br> JUnit4의 @BeforeClass |
| @AfterAll          | 현재 클래스에서 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`가 적힌 각각의 메소드가 실행된 후에 실행<br> JUnit4의 @AfterClass  |
| @Nested            | 중첩된 테스트 클래스임을 알림                                                                                                                    |
| @Tag               | 테스트 필터링을 위한 태그를 선언                                                                                                                 |
| @Disabled          | 테스트 클래스나 메서드를 비활성화<br> JUnit4의 @Ignore                                                                                           |
| @Timeout           | 주어진 시간을 초과하면 테스트 실패                                                                                                               |

@BeforeAll과 @BeforeEach는 글로만 보면 헷갈릴 수 있는데 아래의 코드와 실행결과를 보면 느낌이 올 것이다.  
@BeforeAll이 붙은 메소드는 static 메소드 여야 한다(클래스의 생명주기를 클래스로 변경해도 된다)

```java
import org.junit.jupiter.api.*;

//@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class BeforeAfterTest {
    @BeforeAll
    static void beforeAll() {
        System.out.println("before all");
    }

    @AfterAll
    static void afterAll() {
        System.out.println("after all");
    }

    @BeforeEach
    void beforeEach() {
        System.out.println("before each");
    }

    @AfterEach
    void afterEach() {
        System.out.println("after each");
    }

    @Test
    void test1() {
        System.out.println("Test 1");
    }

    @Test
    void test2() {
        System.out.println("Test 2");
    }

    @Test
    void test3() {
        System.out.println("Test 3");
    }
}

```

실행결과

```
before all
before each
Test 1
after each
before each
Test 2
after each
before each
Test 3
after each
after all
```

@BeforeEach, @AfterEach: 각 테스트가 시작되기 전후에 매번 실행
@BeforeAll, @AfterAll: 클래스에 존재하는 모든 테스트가 실행되기 전과 후에 실행

# 참고 문서

[Junit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
