# 증상

jacoco에서 Lombok의 @Data 주석이달린 도메인 클래스의 코드 커버리지가 0%로 나오는 경우가 발생하였다.

```java
@Entity
@Getter
@NoArgsConstructor
@EqualsAndHashCode(of = "id")
public class Product {
    //...
}
```

<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok%20code%20coverage/product%20before.png?raw=true" width="70%">

# 해결법

lombok에서 생성한 코드에 @lombok.Generated를 붙이면 되는데
[lombok.config](https://projectlombok.org/features/configuration)을 추가하여 여러가지 설정을 할 수 있도록 하고 있다.  
`lombok.addLombokGeneratedAnnotation = true`를 추가하여 롬복에서 생성한 코드에 @lombok.Generated를 추가해주는 설정을 하면 된다.

<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/lombok%20code%20coverage/product%20after.png?raw=true" width="70%">
