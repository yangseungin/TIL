# Virtual DOM

리액트는 렌더링 성능을 위해 가상돔을 사용함  
브라우저에서 돔을 변경하는것은 오래걸리는 작업임  
그래서 랜더링을 빠르게 하기위해 돔 변경을 최소화하는게 좋음  
리액트는 메모리에 가상돔을 올려놓고 이전과 이후의 가상돔을 비교하여 변경된 부분만 실제돔에 반영함

# 리액트 가상돔 반영과정

렌더단계
실제 돔에 반영할 변경사항을 파악하는 단계
가상돔을 이용(가상돔은 리액트 요소로부터 만들어짐)
리액트는 렌더링을 할떄마다 가상돔을만들고 이전의 가상돔과 비교하는데 변경사항을 최소화하기 위해서이다

커밋단계
파악된 변경사항을 실제 돔에 반영하는 단계

리액트 요소 트리를 실제돔으로 만들려면 모든 리액트 요소의 type 속성값이 문자열이여야 함(중간에 컴포넌트가 들어있으면 안됨)

실제 돔을 만들수 있는 리액트 요소 트리를 가상돔이라고 한다.

# 참고 문서

[react doc](https://ko.reactjs.org/docs/getting-started.html)