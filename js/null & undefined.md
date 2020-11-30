# null과 undefined

이 두 타입의 값은 자바스크립트를 사용할 때 조심히 사용해야 하는 값들이다. 둘 다 변수에 값이 없음을 나타내는데 둘의 의미는 다르다.

# null

null은 자바스크립트의 primitive 값 중 하나로 null 또는 빈 값을 표현한다.  
클로저를 이해하려면 자바스크립트의 렉시컬 스코프를 이해해야 하는데 자바스크립트는 컴파일 단계에서 소스코드 문자열을 분석하여 의미를 부여하는 렉싱이라는 작업을 하게 되고 이때 스코프가 된다.

```javascript
//정의된 적도 없고 초기화된 적도 없음
foo; // ReferenceError: foo is not defined

//foo는 존재하지만, 값과 타입이 없다.
let foo = null;
console.log(foo); // null
console.log(typeof foo); // object
```

# undefined

undefined는 전역 스코프의 변수로 primitive 값이며 변수를 선언 후 값을 할당받기 전이면 undefined를 반환한다. undefined는 예약어가 아니므로 식별자로 사용하여 값을 할당할 수 있으나 유지보수와 디버깅이 어려울 수 있으니 피해야 한다.

```javascript
var undefined = true;
if (undefined) {
  console.log("can be used..."); //출력된다.
}
```

## 일치연산과 undefined

변수가 값을 가지는지 확인하기 위해 일치연산자(===)를 사용할 수 있다. 이때 균등연산자(==)는 foo가 null인 경우 출력되기 때문이다.

```javascript
var foo;
if (foo === "undefined") {
  //실행됨
}
```

## typeof 연산자와 undefined

typeof를 사용할 수도 있는데 typeof를 사용하면 변수를 선언하지 않아도 에러를 던지지 않는다.

```javascript
var foo;
if (typeof foo === "undefined") {
  //실행됨
}

if (typeof bar === "undefined") {
  // 오류 없이 true로 보고 실행됨
}

if (bar === undefined) {
  //ReferenceError: bar is not defined
}
```

# null과 undefine의 차이

null: 변수를 선언하고 'null'이라는 빈값을 할당한 경우로 타입은 'object'이다.  
undefined: 변수를 선언하였지만, 값을 할당하지 않은 상태로 타입을 확인해보면 'undefined'이다.

# 참고문서

You don't know JS 타입과 문법, 스코프와 클로저 - 카일 심슨
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/null
