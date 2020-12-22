# JVM이란 무엇인가?

JVM은 Java Virtual Machine의 약자로 자바 바이트 코드를 OS에 맞게 해석하여 실행하게 해주는 역할을 합니다. 즉 사용자의 OS에 종속적이지 않고 어떤 OS를 사용하든지 JVM이 설치되어 있다면 프로그램의 변경 없이 실행이 가능합니다.

<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/JVM.png?raw=true" width="50%">

### JVM의 구성

JVM은 다음과 같이 구성됩니다.

1. 자바 인터프리터(interpreter)
   - 컴파일러에 의해 변경된 바이트코드를 읽고 해석하는 역할
2. 클래스 로더(class loader)
   - 동적으로 클래스를 로딩해주는 역할
3. JIT 컴파일러(Just-In-Time compiler)
   - 컴파일러에 의해 생성된 바이트 코드를 런타임에 기계어로 변환하는 역할
4. 가비지 컬렉터(garbage collector)
   - 사용하지 않는 메모리를 자동으로 회수하여 자원관리를 해주는 역할

# 컴파일이란?

사람이 읽을 수 있는 프로그래밍 언어를 컴퓨터가 이해할 수 있도록 기계어로 변환하는 과정으로 자바에서 소스파일(.java)를 클래스 파일(.class)로 변환하는 과정을 말한다.

### 컴파일과 실행 방법

많은 개발자들이 IDE를 사용하고있기에 직접 컴파일해볼 일이 드물지만 메모장으로 코드를 작성하고 실행해볼 수도 있으니 방법을 확인해봅시다. 우선 Hello 클래스를 작성합니다.

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

이후 터미널이나 cmd에서 `javac` 명령어를 사용하여 컴파일합니다.

```
javac Hello.java
```

그러면 Hello.class 파일이 생성된 것을 확인할 수 있으며 'java' 명령어로 실행할 수 있습니다.

```
java Hello
```

# 바이트 코드란 무엇인가?

JVM이 실행하는 명령어의 형태로 변환된 자바 소스 코드이며 확장자는 .class 입니다. 명령어의 크기가 1Byte여서 바이트코드로 불리며 JVM에 의해 실행되기 때문에 OS에 종속적이지 않고 여러 환경에서 실행 가능합니다.

# JIT 컴파일러란 무엇이며 어떻게 동작하는지

JIT 컴파일러는 Just-In-Time의 약자로 프로그램이 실행되는 시점에 기계어로 변환해 주는 컴파일러입니다.
실행 시점에 인터프리터 방식으로 기계어 코드를 생성하고 캐싱하여 동일한 코드가 또 있을 경우 다시 번역하지 않고 캐싱해둔 값을 사용하여 프로그램의 실행 속도를 향상했습니다.

# JVM 구성 요소

사용하지 않는 메모리를 자동으로 회수하여 자원관리를 해주는 역할
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/JVM2.png?raw=true" width="80%">  
 출처: [위키백과](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0)

## Class Loader

런타임 시 클래스 파일을 Runtime Data Area로 로드하는 작업을 하는 모듈이다.

## Runtime Data Area

JVM이 프로그램을 수행하기 위해 운영체제로부터 할당받은 메모리 공간으로 Method Area, Heap, JVM stack , PC Register, Native Method Stack으로 구분된다.

- Method Area: 클래스를 메모리에 올릴 때 초기화되는 대상을 저장하기 위한 영역
- Heap: new로 생성된 객체나 배열을 저장하는 영역
- JVM Stack: 각종 변수나 임시 데이터, 메소드 정보 등을 저장하는 영역
- PC Register: 스레드가 생성될때마다 생성되며 수행 중인 명령의 주소 값을 갖는 영역
- Native Method Stack: 네이티브 방식을 사용하는 코드(Java가 아닌 C언어)가 쌓이는 영역

## Execution Engine

Runtime Data Area에 배치된 클래스를 실행하는 역할을 수행하며 JIT 방식을 사용한다.
또한 더 이상 사용하지 않는 객체를 해제해주는 Garbage Collector가 존재한다.

# JDK와 JRE의 차이

JRE(Java Runtime Environment)는 자바 실행환경의 약자로 JVM이 프로그램을 동작시킬 때 필요한 라이브러리나 기타 파일들을 포함하고 있다.  
JDK(Java Development Kit)는 자바 개발도구의 약자로 JRE와 개발을 위한 도구들을 포함하고 있다.

# 참고 문서

Java의 정석 - 남궁 성  
http://www.tcpschool.com/java/intro
