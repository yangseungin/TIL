# JPA란?
Java Persistence API의 약자로 JAVA의 ORM 표준으로 인터페이스이다.  
인터페이스이기 때문에 여러 구현체들이 있다(Hibernate, EclipseLink 등등)

# ORM이란?
ORM(Object Relational Mapping)은 약자 그대로 객체와 관계형 데이터베이스를 매핑한다는 뜻이다.  
SQL 쿼리가아닌 메서드를 통해 SQL을 생성하여 데이터를 조작할 수 있다.  
대중적인 언어에는 대부분 ORM이 있다. (Python, TypeScript 등등)


# JPA 동작
<img src="https://github.com/yangseungin/TIL/blob/master/jpa/%EC%82%AC%EC%A7%84/jpa%20%EB%8F%99%EC%9E%91.png?raw=true" width="70%">

JPA는 애플리케이션과 JDBC 사이에서 동작하며 JPA를 사용하면 JPA가 쿼리를 생성하고 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다.

## 저장
<img src="https://github.com/yangseungin/TIL/blob/master/jpa/%EC%82%AC%EC%A7%84/%EC%A0%80%EC%9E%A5.png?raw=true" width="70%">

MemberDAO에서 객체를 저장하고 싶을 때 JPA를 사용하여 객체를 저장한다.  
JPA는 객체를 분석하여 Insert SQL을 생성하고 JDBC API를 사용하여 DB에 SQL를 날린다.


~~~ java
em.persist(member); //em은 EntityManager이다.
~~~


## 조회 
<img src="https://github.com/yangseungin/TIL/blob/master/jpa/%EC%82%AC%EC%A7%84/%EC%A1%B0%ED%9A%8C.png?raw=true" width="70%">

객체를 조회하고 싶을 때 pk 값을 JPA에 넘긴다.  
JPA는 Select SQL을 생성하고 JDBC API를 사용하여 DB에 SQL를 날린다.  
받아온 결과를 객체에 매핑한다.

~~~ java
em.find(Member.class, id); 
~~~

## 수정
객체를 변경하면 엔티티 매니저가 이를 감지하여 Update SQL을 생성하고 JDBC API를 사용하여 DB에 SQL를 날린다.
~~~ java
em.setAge(25); 
~~~

## 삭제
조회와 마찬가지로 Delete SQL을 생성하고 JDBC API를 사용하여 DB에 SQL를 날린다.
~~~ java
em.remove(member);
~~~



# JPA를 사용해야하는 이유?

1. 생산성이 높다.
    - 반복적인 코드와 CRUD용 쿼리를 직접 작성하지 않아도 된다.
2. 유지보수가 쉽다.
    - 필드의 변경과 같은 일이 생겨도 쿼리를 일일이 수정하지 않아도 된다.  
3. 패러다임의 불일치 해결
    - 상속
        - 객체는 상속관계가 있으나 RDB는 없다.
    - 연관관계
        - 객체는 참조(Reference)를 사용하여 연관관계를 찾으며 단방향으로만 조회할 수 있고 RDB는 외래키를 사용하여 연관관계를 찾으며 양방향으로 조회 가능하다.
    - 객체 그래프 탐색
        - 객체는 자유롭게 객체그래프를 탐색할수 있어야 하지만 모든 연관된 그래프를 메모리에 올려둘수는 없다. 이를 지연로딩을 통해 해결하였다.
    - 비교하기 차이
        - RDB는 row를 통해 구분을 하는 반면 객체는 동일성과 동등성이 있다.
        - 같은 트랜잭션에서 컬렉션에서 조회하는것처럼 동일성을 보장한다.
4. 성능상의 이점
    - 캐싱기능
        - 트랜잭션 내에서 같은회원을 두번 조회할경우 데이터베이스와 두번통신하지 않고 같은 엔티티를 반환하여 약간의 조회 성능을 향상시킨다.(큰차이 없다고 한다.)
    - 트랜잭션을 지원하는 쓰기 지연
        - 트랜잭션을 commit할때까지 insert sql을 모은 후 JDBC BATCH SQL 기능을 사용하여 한번에 전송한다. 
    - 지연 로딩
        - 객체가 실제로 사용될 떄 로딩한다.
5. 데이터베이스 벤더 독립성
    - 특정 데이터베이스 기술에 종속되지 않도록 해준다.
    - (페이징: Oracle-rownum, MySQL- limit)