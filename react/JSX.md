# JSX 이해하기

JSX는 `Javascript XML`의 약자로 React 컴포넌트에서 HTML 마크업을 그대로 사용할 수 있게 해주며 Javascript의 확장이다.  
특히 react에서는 React.createElement()호출을 반복해야 하는 불편함을 해소해주며 레퍼런스에서도 JSX를 소개할 때 JSX 사용을 권장한다고 한다.

## 중괄호 내에 자바스크립트 표현

JSX에서 javascript를 표현할 때는 `중괄호{}`로 감싸야 된다.

```javascript
const name = "yang";
const div = <div>Hello {name}</div>;

ReactDOM.render(div, document.getElementById("root"));
```

## 여러 엘리먼트 사용 시 하나의 엘리먼트로 감싸야 함

```javascript
function App() {
    return (
        <div>
            Hello
        </div>
        <div>
            World
        </div>
    )
}

export default App;
```

감싸지 않는 경우 하나의 태그로 감싸야 한다고 에러가 발생한다.  
<img src="https://github.com/yangseungin/TIL/blob/master/react/picture/tag%20error.png?raw=true" width="80%">

해결하려면 div 태그로 감싸주거나 Fragment를 사용하여 감싸면 된다. (축약형으로 <>로 작성하였다.)

```javascript
function App() {
  return (
    //<>는 Fragment의 축약형
    <>
      <div>Hello</div>
      <div>World</div>
    </>
  );
}

export default App;
```

## 닫는 태그를 반드시 써야 함

```javascript
function App() {
  return <div></div>; //혹은 <div/>
}
```

##

리액트에서는 JSX로 작성된 코드를 createElement함수를 호출하는 코드로 변환하기 위해 바벨을 사용한다.

```javascript
//jsx
var div = <div className='hello'>hello world</div>;
```

```javascript
//createElement를 사용하는 코드
"use strict";

var div = React.createElement("div", { className: "hello" }, "hello world");
```

React.createElement는 다음과 같은 객체를 생성한다

```javascript
const div = {
  type: "div",
  props: {
    className: "hello",
    children: "hello world",
  },
};
```

# 참고 문서

[react doc](https://ko.reactjs.org/docs/jsx-in-depth.html)
