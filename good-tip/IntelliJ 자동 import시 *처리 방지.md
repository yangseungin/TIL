# 증상

IntelliJ에서 자동 import나 Optimize imports를 할 떄 개별 클래스가 아니라 와일드카드로 전체가 import되는 현상이 발생하게 된다.  
import \* 은 좋지 못한 습관이니 이를 방지해보자.

# 해결법

`preferences - Editor - Code Style - java`에 가보면 `Class count to use import with '*'`과 `Names count to use static import with '*'`의 기본값이 5와 3으로 낮게 잡혀있는것을 확인할 수 있다.  
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/auto%20import/auto%20import%20wildcard.png?raw=true" width="70%">

이값을 999로 크게 늘려 준후 확인해보면 \*로 import되지 않고 개별 import가 되는것을 확인할 수 있다.

<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/auto%20import/auto%20import%20wildcard%20%EB%B3%80%EA%B2%BD%20%ED%9B%84.png?raw=true" width="70%">
