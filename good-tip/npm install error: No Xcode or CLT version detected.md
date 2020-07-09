webpack 실습을 위해 npm을 초기화하고 webpack-cli를 설치하는 도중 다음과 같은 에러가 발생하였다.

~~~
npm init -y
npm install webpack webpack-cli --save-dev

//생략

gyp: No Xcode or CLT version detected!
...
~~~
말 그대로 Xcode나 CLT 버전이 감지되지 않았다는 뜻인데 최근에 Xcode를 앱스토어에서 설치 도중 문제가 있었는데 그게 문제인듯하여 삭제 후 다시 설치하였다.

1. xcode 경로확인
    ~~~
    xcode-select --print-path
    ~~~
2. xcode 삭제(위에서 확인한 경로)
    ~~~
    sudo rm -rf /Library/Developer/CommandLineTools
    ~~~
3. xcode 설치
    ~~~
    xcode-select --install
    ~~~

Xcode를 다시 설치한 후 npm install을 하니 성공적으로 설치가 된 것을 확인하였다.