brew를 통해 node를 설치하기위해 아래와같은 명령어를 하였으나

~~~
brew install node
~~~

하지만 설치는 성공적으로 되지않았고 에러가 발생.

~~~
Error: The brew link step did not complete successfully The formula built, but is not symlinked into /usr/local Could not symlink share/doc/node/gdbinit /usr/local/share/doc/node is not writable. You can try again using: brew link node
~~~

아래와 같이 링크를 하라고 하였으나 여지없이 에러를 뱉어냄  
<img src="https://github.com/yangseungin/TIL/blob/master/%EC%9E%A1%EB%8B%A4%ED%95%9C%ED%8C%81/%EC%82%AC%EC%A7%84/npm%20install1.png?raw=true" width="70%">


권한으로 인해 발생한 문제이니 권한을 바꿔주고 실행하면 된다. (sudo chown -R $(whoami) /usr/local 로 하라는 글도 많이 보였으나 동작하지 않아 아래와같이 하였다.)

~~~
sudo chown -R $(whoami) $(brew --prefix)/*
~~~

<img src="https://github.com/yangseungin/TIL/blob/master/%EC%9E%A1%EB%8B%A4%ED%95%9C%ED%8C%81/%EC%82%AC%EC%A7%84/npm%20install2.png?raw=true" width="70%">
