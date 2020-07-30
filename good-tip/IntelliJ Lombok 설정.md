IntelliJ에서 라이브러리를 사용하기

# Lombok이란?
자바 라이브러리로 일반적으로 자바 개발을 하면서 Model 객체를 만들 때 생성자, 필드에 대한 Getter, Setter와 같이 반복적으로 만드는 코드를 어노테이션으로 줄여준다.
롬복 홈페이지에서는 아래와 같이 표현하였다.
~~~
Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
~~~
# Lombok을 사용하는 이유?
- 생성자, getter, setter를 추가하기위한 코드의 길이가 상당히 줄어든다
~~~ java
//적용전
public class Person {
    private String name;
    private int age;
    private String location;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getLocation() {
        return location;
    }

    public void setLocation(String location) {
        this.location = location;
    }
}
~~~
~~~ java
//lombok 적용
@Getter @Setter
public class Person {
    private String name;
    private int age;
    private String location;
}
~~~
- 코드가 줄어들어 가독성과 생산성이 높아진다.

# Lombok 적용하기
jar파일로 설치하여도되지만 Intellij 기준으로 정리하였다.

## 1. 의존성 추가하기
1. Spring Initializr  
Intellij Ultimate 버전에서는 프로젝트를 생성할때 Spring Initializr을 사용하여 생성할수 있다.(http://start.spring.io 에서 의존성을 추가하여도 된다.)  
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok/spring%20initializr.png?raw=true" width="70%">
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok/spring%20initializr2.png?raw=true" width="70%">

2. Gradle 사용시  
build.gradle에 의존성을 추가 한다.
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok/gradle.png?raw=true" width="70%">

3. Maven 사용시  
pom.xml에 의존성을 추가한다.
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok/maven.png?raw=true" width="70%">


## 2. 플러그인 설치
Lombok 플러그인을 설치 후 IntelliJ 재시작
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok/plugin.png?raw=true" width="70%">

## 3. Intellij Annotation Processing 설정
Preferences - Build, Execution, Deployment - Annotation Processors에서 Enable annotation processing 을 체크한다.
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok/annotation%20processing.png?raw=true" width="70%">

이 설정은 Lombok을 사용하는 프로젝트마다 설정해줘야 한다. (설정하지 않을시 정상 동작하지 않는다.)
