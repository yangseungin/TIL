# create-react-app이란

공식 레퍼런스에서는 SPA 앱을 만들때 `Create React App`을 추천하고 있다.

# 스크립트

## Start

개발모드로 실행이며, 코드 변경시 페이지에 자동으로 리로드 된다.  
콘솔에 빌드 오류나 린트 경고를 출력하며 최적가 되지 않아 배포할때가 아닌 개발할때만 사용한다.

## build

배포할때 사용, 정적파일 생성, SSR 불가,

작은이미지는 js에 내장되고 큰이미지는 media디렉토리에 있음

## test

jest를 테스트 러너로 사용함.
`.spec.js, .test.js`로 끝나는 파일이나 `__tests__` 밑에 있는 파일은 모두 테스트 파일이 됨.

## eject

react-scripts를 사용하지않고 모든 설정파일을 추출하는 명령

## create-react-app 업데이트

react의 [changelog](https://github.com/facebook/react/blob/main/CHANGELOG.md#1702-march-22-2021)를 보면 자주 변경되는것을 볼 수 있다.  
create-react-app은 두개의 패키지로 나뉘는데 `create-react-appp`과 `react-scripts`이다.  
create-react-app은 새 프로젝트를 만들떄 사용하며 react-scripts는 생성된 프로젝트의 개발 종속성이다.
이중에서 create-react-app에 기능이 추가되었다고 할 때 react-scripts 버전만 올리면 되는데 eject로 추출시 수동으로 설정파일을 수정해야 한다.

# polyfill

polyfill을 추가할떄는 core.js를 사용함

# 환경변수

process.env.{변수이름}  
process.env.NODE_ENV  
npm start실행 -> development  
npm test실행 -> test  
npm run build실행 -> production

환견변수가 많아지면 .env로 관리하는게좋음(루트폴더에서)

# 참고 문서

[react doc](https://ko.reactjs.org/docs/jsx-in-depth.html)
