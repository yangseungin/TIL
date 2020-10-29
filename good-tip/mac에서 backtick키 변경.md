es6에서 표현식을 템플릿 리터럴은 backtick(\`)으로 묶어서 사용하는데 macOS Sierra이후부터 숫자키 1 왼쪽의 backtick(\`)이 원화(₩)로 바뀌었다.  
키를 변경하려면 아래와 같이 진행하면 된다.

1. ~/Library 디렉토리 밑에 KeyBindings 생성

```
mkdir ~/Library/KeyBindings
```

2. KeyBindings 디렉토리 안에 DefaultkeyBinding.dict 생성

```
touch DefaultkeyBinding.dict //혹은 vi DefaultkeyBinding.dict
```

3. 생성된 파일에 아래 내용 추가

```
{
 "₩" = ("insertText:", "`");
}
```

실행 중인 애플리케이션을 새로 실행하면 변경됨을 확인할 수 있다.
