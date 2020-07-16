Repository에서 @Query어노테이션을 통해 Native Query를 사용시 nativeQuery 옵션을 true로 줘야한다(default 값은 false이다.)  

~~~ java
@Query(value = "SELECT userId, userName FROM Member", nativeQuery = true)
    public List<Object[]> listMember();
~~~
네이티브 쿼리는 위치기반 파라미터가 0부터 시작하며 다중 컬럼일 경우에는 Object []로 받아서 컬럼 지정 순서대로 값을 뽑아서 사용한다.