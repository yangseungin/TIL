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
자바 9에서는 두 가지 방법으로 맵을 초기화할 수 있다.
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

# 맵 처리
자바 8부터 Map 인터페이스에 몇가지 디폴트 메서드가 추가됐다.

## foreach()
키와 값을 인수로 하는 Biconsumer를 인수로 받는 forEach메서드를 활용해서 확인할 수 있다.
```java
Map<String, String> user = Map.of("hodong123", "김호동", "gildong443", "홍길동", "tomsday", "tom");
user.forEach((id, name) -> System.out.println(id + " / " + name));
```

## 정렬 메서드
Entry.comparingByValue, Entry.comparingByKey를 넘겨 맵을 키와 값을 기준으로 정렬 할 수 있다.
- comparingByValue: value 기준으로 정렬
- comparingByKey: key 기준으로 정렬
```java
Map<String,String> favoriteFruit = Map.ofEntries(
        entry("홍길동", "사과"),
        entry("김계란", "바나나"),
        entry("강호동", "파인애플")
);
favoriteFruit.entrySet()
        .stream()
        .sorted(Map.Entry.comparingByKey())
        .forEachOrdered(System.out::println);
```
실행 결과
```
강호동=파인애플
김계란=바나나
홍길동=사과
```

## getOrDefault()
기존에는 찾으려는 키가 없는 경우 null이 반환되는데 getOrDefault메서드는 키가 없으면 기본값을 반환할 수 있다.

```java
System.out.println(favoriteFruit.getOrDefault("이수근","키위")); // 키위 출력
```

## 계산 패턴
Map에 key가 존재하는지에 따라 어떤 동작을 실행하고 결과를 저장해야 하는지 상황일 때 사용하는 패턴
- computeIfAbsent: 제공된 키에 해당하는 값이 없거나 null이면 , key를 이용해 새값을 계산하고 맵에 추가
  (정보를 캐시할 때 사용)
- computeIfPresent: 제공된 키가 존재하면 새값을 계산하고 맵에 추가
- compute: 제공된 키로 새값을 계산하고 맵에 추가

## 삭제 패턴
key가 특정한 값과 연관되었을 때만 제거하는 오버로드 버전 메서드
```java
Map<String,String> like = new HashMap<>();
like.put("강호동", "사과");
like.put("이수근", "파인애플");
like.put("김계란", "바나나");
System.out.println(like);
like.remove("김계란", "다른과일");
System.out.println(like);
like.remove("김계란", "바나나");
System.out.println(like);
```
실행 결과
```
{강호동=사과, 이수근=파인애플, 김계란=바나나}
{강호동=사과, 이수근=파인애플, 김계란=바나나}
{강호동=사과, 이수근=파인애플}
```

## 교체 패턴
map의 항목을 바꾸는 데 사용할 수 있는 메서드
- replaceAll: BiFuction을 적용한 결과로 각 항목의 값을 교체
- replace: 키가 존재하면 값을 바꿈, 키가 특정 맵으로 매핑 되었을때만 교체하는 메서드도 존재
```java
like.replace("강호동", "키위");
like.replace("강호동", "키위", "멜론");
like.replaceAll((name, fruit) -> "상태 좋은 " + fruit);
```
## 합침
putAll()  
- 특정 맵의 데이터를 모두 put
- 중복된 키는 덮어씀
```java
// base data
Map<String,String> favoriteMovie = new HashMap<>();
favoriteMovie.put("아빠","스타워즈");
favoriteMovie.put("엄마","해리포터");
favoriteMovie.put("애기","반지의제왕");
Map<String,String> dislikedMovie = new HashMap<>();
dislikedMovie.put("아빠","스파이더맨");
dislikedMovie.put("엄마","배트맨");
dislikedMovie.put("할아버지","앤트맨");

favoriteMovie.putAll(dislikedMovie); // {아빠=스파이더맨, 애기=반지의제왕, 할아버지=앤트맨, 엄마=배트맨}
```
merge()
- 중복된 키를 어떻게 합칠지 결정할 수 있음
```java
favoriteMovie.forEach((key, value) -> dislikedMovie.merge(key,value,(movie1, movie2) -> movie1 + " & "+movie2));
System.out.println(dislikedMovie);  // {아빠=스파이더맨 & 스타워즈, 애기=반지의제왕, 할아버지=앤트맨, 엄마=배트맨 & 해리포터}
```

# 개선된 ConcurrentHashMap
- 동시성 친화적이며 최신 기술을 반영한 HashMap
- 특정 부분만 잠궈 동시에 추가, 갱신 작업이 가능
- 동기화된 HashTable 버전에 비해 읽기, 쓰기 연산이 월등함



# 참고문서

모던 자바 인 액션 - 라울 게이브리얼 우르마, 마리오 푸스코, 앨런 마이크로소포트
