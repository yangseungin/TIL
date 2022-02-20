# 컬렉션 팩토리
아래 방법은 정수의 리스트를 선언하는 방법이다.

```java 
List<Integer> nation = new ArrayList<>();
        nation.add(1);
        nation.add(2);
        nation.add(3);
        nation.add(4);

//Arrays.asList()사용시 고정크기의 리스트
List<Integer> numbers = Arrays.asList(1,2,3,4);
        numbers.set(0,10); // 갱신 가능
        numbers.add(5); //요소의 추가나 삭제 불가능
```
팩토리 메서드의 특징
- 요소 변경 불가
- null 요소를 금지

Arrays.asList() 팩토리 메서드를 사용시 고정크기의 배열로 구현되어 있기 떄문에 UnsupportedOperationException을 발생시킨다.

## 리스트 팩토리
List.of()를 사용해서 간단하게 리스트를 만들 수 있다.
```java
List<Integer> oddNumbers = List.of(1,3,5,7,9);
```
List.of는 변경할수 없는 리스트이다. (immutable)  
수정, 추가, 삭제 모두 불가능하며 null은 요소로 올 수 없다.

## 집합 팩토리
Set.of()를 사용하여 간단하게 set을 만들 수 있다.
```java
Set<Integer> integers = Set.of(1, 2, 3);
```
요소로 중복된 값을 넘기면 IllegalArgumentException이 발생한다.

## 맵 팩토리
Map.of()를 사용하여 map을 만들 수 있다.  
자바 9에서는 두가지방법으로 맵을 초기화할 수 있다.
```java
Map<String, Integer> friends = Map.of("hodong", 24, "tom", 27);

Map<String, Integer> friends2 = Map.ofEntries(Map.entry("hodong", 24)
        , Map.entry("tom", 27));
```
Map.ofEntries는 인수로 Map.Entry<K,V>를 받는다.

# 리스트와 집합 처리
새로운 결과를 만드는 스트림과 달리 removeIf, replaceAll은 호출한 컬렉션을 바꾼다.

```java
List<Integer> collect = IntStream.rangeClosed(1, 50).boxed().collect(Collectors.toList());
collect.removeIf(integer -> integer < 40);
System.out.println(collect);    // 40~50 출력

List<Integer> from1to50 = IntStream.rangeClosed(1, 50).boxed().collect(Collectors.toList());
from1to50.replaceAll(integer -> integer+50);
System.out.println(from1to50); // 51~ 100 출력
```

# 참고문서

모던 자바 인 액션 - 라울 게이브리얼 우르마, 마리오 푸스코, 앨런 마이크로소포트
