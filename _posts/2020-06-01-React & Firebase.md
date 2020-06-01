# React & Firebase 



 BAAS(Backend as a service)로써 앱(iOS, Android) 및 웹의 백엔드 개발을 서포팅하기 위해 개발된 서비스로 프론트에 집중하며 빠르게 앱을 개발할 수 있도록 지원해주는 툴입니다.

 데이터베이스, 사용자인증(oAuth), 클라우드, 호스팅, 오류보고, 에드워즈, 에드몹, 애널리틱스와 같은 수많은 서비스를 제공합니다.

## 관련강좌

_________

1. [인프런 파이어베이스 강좌(무료)](#[https://www.inflearn.com/course/%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B0%95%EC%A2%8C-%EC%9B%B9-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98/](https://www.inflearn.com/course/파이어베이스-강좌-웹-어플리케이션/))
2. [파이어베이스 Docs](#https://firebase.google.com/docs/?authuser=0)
3. [React & Firebase(영문)](#https://www.codementor.io/yurio/all-you-need-is-react-firebase-4v7g9p4kf)
4. [Firecast - React & Firebase (영문 유튜브)](#https://www.youtube.com/watch?v=mwNATxfUsgI)
5. [Firebase 유튜브 채널 (영문)](#https://www.youtube.com/user/Firebase)
6. [Firebase 와 Redux로 채팅앱 만들기 (영문)](#https://medium.com/react-native-development/build-a-chat-app-with-firebase-and-redux-part-1-8a2197fb0f88)
7. [Firebase blog - React Native & Firebase (영문)](#https://firebase.googleblog.com/2016/01/the-beginners-guide-to-react-native-and_84.html)
8. [Realm - Firebase 소개](#https://academy.realm.io/kr/posts/firebase-as-a-real-mobile-backend/)
9. [슬라이드 쉐어 - Firebase 호스팅](#https://www.slideshare.net/sungbeenjang/firebase-for-web-1-hosting)
10. [React Native & Firebase & You (영문)](#https://blog.callstack.io/react-native-firebase-and-you-a07ae507910)
11. [React - Redux - Firebase (영문)](#http://react-redux-firebase.com/)



## 시작하기

______

### 리액트설치

CRA(Create React App)을 이용해서 리액트를 설치합니다.[관련 가이드](#https://github.com/facebook/create-react-app)

```shell
$ npm install -g create-react-app

$ create-react-app my-app
$ cd my-app/
$ npm start (또는 yarn start)
```

결과화면

![](https://user-images.githubusercontent.com/41600558/83378526-e2862f00-a413-11ea-98d5-13a652eba963.png)

서버를 닫고 `yarn build` 명령어로 스태틱파일로 컴파일 후, 생성된 `build`폴더를 파이어베이스 기본폴더로 등록해야 한다.

### 파이어베이스 CLI 설치

파이어베이스를 프로젝트에 설치하기 위해서는 Command Line Tool을 사용해야 합니다.

npm 플러그인 설치

```sh
$ npm i -g firebase-tools
```

### 파이어베이스 등록

[구글 파이어베이스](#[https://firebase.google.com/?utm_source=google&utm_medium=cpc&utm_campaign=1001467%20%7C%20Firebase*%20Brand%20GENERIC%20%7C%20Global%20%7C%20en%20%7C%20Desk%2BTab%2BMobile%20%7C%20Text%20%7C%20BKWS%20%5B2017%5D&utm_term=%7Bkeyword%7D&gclid=CjwKCAiA9f7QBRBpEiwApLGUittjTT25Om4ihtPBy4LDG7M_3vvsUY5lqqWXNkPorPKq1bc7vg-DvhoCTPkQAvD_BwE](https://firebase.google.com/?utm_source=google&utm_medium=cpc&utm_campaign=1001467 | Firebase* Brand GENERIC | Global | en | Desk%2BTab%2BMobile | Text | BKWS [2017]&utm_term={keyword}&gclid=CjwKCAiA9f7QBRBpEiwApLGUittjTT25Om4ihtPBy4LDG7M_3vvsUY5lqqWXNkPorPKq1bc7vg-DvhoCTPkQAvD_BwE))에 접속해서 '프로젝트'를 추가합니다.

![](https://user-images.githubusercontent.com/41600558/83378797-d9e22880-a414-11ea-9cbc-06f85797cbb4.png)

해당 프로젝트로 이동하여 Hosting 서비스를 시작합니다. '시작하기'를 클릭하면 가이드에 따라 진행하시면 됩니다.

### 파이어베이스 리액트 프로젝트에 설치

터미널에서 `firebase login` 명령어로 파이어베이스를 사용할 구글 계정을 등록합니다.(프로젝트를 생성했던 계정으로 로그인하셔야 합니다) 리액트 프로젝트에 설치하기 위해 리액트 폴더에서 `firebase init`을 실행합니다.

![](https://user-images.githubusercontent.com/41600558/83379219-f7fc5880-a415-11ea-9423-368ae707eb77.png)

엔터 클릭하여 계속 진행

![](https://user-images.githubusercontent.com/41600558/83379272-12363680-a416-11ea-8965-c28a2b8db439.png)

원하는 옵션을 `spacebar`와 방향키로 선택 후 `Enter`를 클릭하여 다음 진행

![](https://user-images.githubusercontent.com/41600558/83379350-401b7b00-a416-11ea-8d32-46296502b090.png)

firebase 프로젝트를 이미 생성했으므로 첫 번째 선택 후 생성한 프로젝트 선택

그 다음 데이터베이스 규칙을 지정한 파일을 선택하는 단계가 나오는데 별도의 파일이 없다면 `Enter` 후 진행

![](https://user-images.githubusercontent.com/41600558/83379536-ba4bff80-a416-11ea-9ef4-ff3012d1e8ea.png)

deploy할 폴더를 지정하는 것입니다. 전에 생성했던 `build` 폴더로 수정 후 등록합니다.

다음으로 SPA(Single Page App) 프로젝트 여부를 선택하는 옵션이 나옵니다. 알맞게 선택해주시면 됩니다.

마지막으로 이미 `build`폴더가 생성되어 있어서 overwrite 할 것인지 선택지가 나오는데 `n`을 입력해서 취소합니다.

설치가 완료되었습니다. 이제 로컬에 띄웁니다.

```sh
$ firebase serve
```

올바르게 진행하셨다면 **localhost:5000**에서 파이어베이스 프로젝트가 서빙됩니다. 이전 리액트 실행화면과 동일한 화면이 나와야 합니다.

### 파이어베이스 Deploy

호스팅 기능이 활성화 되었다면, `firebase deploy` 명령어로 실제 서버에 프로젝트를 올릴 수 있습니다.

![](https://user-images.githubusercontent.com/41600558/83379985-c84e5000-a417-11ea-83fe-1d2d54de0e44.png)

deploy를 완료하면 터미널에 사이드 url이 나오는데, 이 url로 접속하시면 본인의 리액트 프로젝트를 볼 수 있습니다.

`yarn build` 명령어 후 `firebase deploy`를 실행해야 하는 번거로움 줄이기 위해 `package.json`파일에 다음과 같이 추가합니다.

```json
"scripts":{
	"deploy" : "react-scripts build && firebase deploy"	
}
```

이제 `npm run deploy` 명령어 한줄로 build부터 deploy까지 자동으로 실행합니다.

### 리액트 & 파이어베이스

파이어베이스의 기능들(데이터베이스, 사용자 인증)을 사용하려면 파이어베이스 자바스크립트 코드를 가져와야 합니다.

`npm firebase`를 통해서 모듈을 설치해줍니다.

```sh
$ npm i -S firebase
```

firebase를 `import`해서 관련된 함수를 작성해주시면 됩니다. 설정하는 방법은 두가지가 있습니다.

1. index.js 파일에서 바로 `import` & `config` 코드를 적는 것
2. 별도의 Firebase.js(예시) 파일을 생성해서 설정 코드를 담아둔 후, `import`해서 사용하는 방법



