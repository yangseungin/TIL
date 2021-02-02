# JUnit5란?

JUnit은 자바에서 단위테스트를 제공하는 테스트 프레임워크로 테스트를 작성하고 실행할 수 있다.
junit5는 공식 문서에서는 3가지의 서브 프로젝트의 모듈로 구성되어있다고 한다.

```
JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage
```

- Platform  
   JVM에서 테스트 프레임워크를 시작하기위한 기초역활
- Jupiter  
   Junit5에서 테스트를 작성과 확장을 위한 새로운 프로그래밍 모델과 확장 모델의 조합
- Vintage  
   하위 버전(JUnit3, JUnit4)을 위한 테스트엔진제공

junit5은 런타임에는 Java 8이상 이 필요하나 이전버전의 JDK로 컴파일된 코드는 테스트를 할 수 있다.

# 테스트 환경 확인

- Ide: IntelliJ Idea Ultimate
- oS: Mac OS Big Sur
- Java8
- Gradle 6.8

## 의존성 추가

사용을 위해서는 의존성을 추가해야 한다. [Maven Repository](https://mvnrepository.com/)에서 의존성을 검색하여 추가할 수 있다. 빌드 도구로는 Gradle을 사용할 것이다.  
프로젝를 아직 생성하지 않았다고 하면 Gradle을 사용하여 프로젝트를 생성할 수 있다.

```
gradle init
```

이때 테스팅 프레임워크를 선택할 수 있는데 JUnit Jupiter를 선택하면 의존성이 추가된채로 프로젝트가 생성된다.

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

# 어노테이션

# 참고 문서

[Junit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
