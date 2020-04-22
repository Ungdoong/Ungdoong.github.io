# React-Native 60+ 버전에서 폰트 적용



 구글 검색하여 나오는 대부분의 폰트 적용 방법으로는 안드로이드와 ios 프로젝트에 해당폰트가 link되지 않은 문제가 발생하여 60+버전 이상에서 폰트를 적용하는 법을 찾아 적용하였다.

## 기존 적용 방법

__________

1. `package.json` 파일에 `rnpm` 입력

   ```json
   "rnpm": {
   	"assets": [
           "./assets/fonts/"
       ]
   }
   ```

2. 적용할 `.ttf`파일을 `assets/fonts`폴더에 복사

   ![](https://user-images.githubusercontent.com/41600558/79982049-9ffe3800-84e0-11ea-89a2-4657929742da.png)

3. 터미널에서 `react-native link` 명령어 입력

   ```bash
   $ react-native link
   ```

4. 사용하고자 하는 요소의 `fontFamily`에 폰트 이름 입력

   ```javascript
   const styles = StyleSheet.create({
     title: {
       fontFamily: "Lovely-Boys",
       color: 'black',
       fontSize: 24,
     },
   });
   ```

### 폰트 관리 방법

 `fonts.js` 파일로 커스텀 폰트 관리

- `fonts.js`

  ```javascript
  export const Fonts = {
      'Lovely-Boys': 'Lovely-Boys',
      BMDOHYEON: 'BMDOHYEON'
  }
  ```

- `HeaderTitle.js` - 

  ```javascript
  import React, {Component} from 'react';
  import {StyleSheet} from 'react-native';
  import {Text} from 'react-native-elements';
  import {Fonts} from '../../fonts'
  
  function HeaderTitle() {
    return <Text style={styles.title}>Foowipe</Text>;
  }
  
  const styles = StyleSheet.create({
    title: {
      fontFamily: Fonts["Lovely-Boys"], // fonts.js에서 불러와 사용
      color: 'black',
      fontSize: 24,
    },
  });
  ```

### `react-native link`가 적용되었는지 확인하는 법

`react-native link`명령어가 올바르게 적용된 경우 

- android

  `android/app/src/main/assets/fonts`폴더에 같은이름의 `ttf`파일이 생성됨

  ![](https://user-images.githubusercontent.com/41600558/79982763-c670a300-84e1-11ea-8791-e34a54bb4e5b.png)

- ios

  `ios/<프로젝트명>/Info.plist`파일에 해당내용이 추가됨

  ```html
  	<key>UIAppFonts</key>
  	<array>
  		<string>BMDOHYEON.ttf</string>
  		<string>Lovely-Boys.ttf</string>
  	</array>
  ```

  

## 60+ 버전의 react-native 프로젝트에서 적용법

1. `package.json`이 존재하는 경로에서 `react-native.config.js` 파일 생성(이미 존재하는 경우 생략)

2. `react-native.config.js` 에 **assets: [‘./assets/fonts/’]** 입력

   ```javascript
   module.exports={
       project:{
           ios: {},
           android: {},
       },
       assets: ['./assets/fonts/']
   }
   ```

3. 터미널에서 `react-native link` 입력

   ```bash
   $ react-native link
   ```

4. 이 후 과정은 기존 방법과 동일

