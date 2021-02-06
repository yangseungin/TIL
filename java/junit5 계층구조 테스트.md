# Describe - Context - It 구조

BDD(Behaviour Driven Development)에서 테스트 코드를 작성하는 구조로 구조에대해선 [Betterspecs](https://www.betterspecs.org/#describe)에 자세히 설명이 되어있다.  
키워드를 간단히 요약해보면

- Describe: 테스트할 대상을 명시
- Context: 언제와 같은 상황을 명시
- It: 대상이 해야 할 행동을 명시

내가 생각한 구조의 가장 큰 장점은 테스트를 계층적으로 작성하여 수행결과를 파악하기 좋다는 점이다. 수행결과가 보기 좋아서 자신이 놓친 경우를 찾기 쉽다.

# Junit5의 @Nested

Junit5부터 `@Nested`를 사용하여 계층적으로 테스트코드를 작성할 수 있다.

# 참고 문서

[Junit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)  
[Betterspecs](https://www.betterspecs.org/#describe)
