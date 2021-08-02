# create-react-app이란

4가지 모드

## Start

개발모드로 실행, 최적화가 적용이 안된상태, 배포할떄 사용하면 안됨.

## build

배포할때 사용, 정적파일 생성, SSR 불가,

작은이미지는 js에 내장되고 큰이미지는 media디렉토리에 있음

## test

## eject

react-scripts를 사용하지않고 모든 설정파일을 추출하는 명령

추출하지않으면 cra에 기능이 추가될떄 react-scripts버전만올리면되는데 추출하면 수동으로 설정파일을 수정해야함(꼭필요한경우가 아니면 추출하지 않는게 좋음)

# polyfill

polyfill을 추가할떄는 core.js를 사용함

# 환경변수

process.env.{변수이름}  
process.env.NODE_ENV  
npm start실행 -> development  
npm test실행 -> test  
npm run build실행 -> production

환견ㅇ변수가 많아지면 .env로 관리하는게좋음(루트폴더에서)
