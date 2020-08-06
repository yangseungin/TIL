보통 DBeaver와 같은 무료 DB툴을 사용하지만 인텔리제이에서도 디비툴을 제공하는데 여기에서 db에 연결을 하였지만, db 구조가 보이지 않는 경우가 있다.  
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/intellij-dbtool/Structure%20is%20invisible.png?raw=true" width="70%">  

이 경우 다음과 같이 설정하면 된다.  

1. Data Source Properties  
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/intellij-dbtool/Data%20Source%20Properties.png?raw=true" width="70%">

2. Schemas 선택  
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/intellij-dbtool/Schemas.png?raw=true" width="70%">

3. 해당 db 선택 후 public 혹은 All schemas 선택  
<img src="https://github.com/yangseungin/TIL/blob/master/good-tip/%EC%82%AC%EC%A7%84/intellij-dbtool/check%20schema.png?raw=true" width="70%">

정상적으로 보이는 것을 확인할 수 있다.