# React-Navigation



화면 별 스택 구성과 화면간 이동에 사용되는 라이브러리



## react-navigation

___________

### 설치

```bash
$ yarn add react-navigation
# or
$ npm install react-navigation
```

- 의존 라이브러리 설치

  ```bash
  $ yarn add react-native-gesture-handler react-native-reanimated react-native-screens react-native-stack
  # or
  $ npm install react-native-gesture-handler react-native-reanimated react-native-screens react-native-stack
  ```

- RN >= 0.60의 경우

  ```bash
  $ cd ios
  $ pod install
  ```

- RN <= 0.59의 경우

  ```bash
  $ react-native link react-native-reanimated
  $ react-native link react-native-gesture-handler
  ```

  

### src/common/NavigationService.js 생성

모듈화 시켜 사용하기 위해 common디렉토리 밑에 NavigationService.js을 생성

```javascript
import { NavigationActions } from 'react-navigation';

let _navigator;

function setTopLevelNavigator(navigatorRef) {
    _navigator = navigatorRef;
}

function navigate(routeName, params) {
    _navigator.dispatch(
        NavigationActions.navigate({
            routeName,
            params,
        })
    )
    
}

function back() { 
    _navigator.dispatch(
        NavigationActions.back()
    );
}

// add other navigation functions that you need and export them

export default {
    navigate,
    setTopLevelNavigator,
    back,
};
```

 

navigate() 함수를 사용해 다른 스크린으로 이동할 수 있고 params값을 이용하여 데이터도 주고받을 수 있습니다.

back() 함수를 이용하면 이전 스크린으로 돌아갈 수 있습니다.

NavigationService.js를 사용하기 위해서 common 밑에 index.js 파일이 있어야 하고 밑에 코드를 넣어주어야 합니다.

```javascript
import _NavigationService from './NavigationService';
export const NavigationService = _NavigationService;
```

 

### **라우터 설정 및 화면 추가**

#### App.js 수정

```javascript
import React from 'react';
import RootRouter from './src/Router';

class App extends React.Component {

  render() {
    return (
      <RootRouter />
    );
  }

}

export default App;
```

 

#### src/Router.js 생성

```javascript
import React, { Component } from 'react';
import Navigation from '../src/navigation';
import { NavigationService } from '../src/common';

class Router extends Component {

    constructor(props) {
        super(props);
    }

    render() {
        return (
            <Navigation
                ref={navigatorRef => {
                    NavigationService.setTopLevelNavigator(navigatorRef);
                }}
            />
        )
    }
}

export default Router;
```

 

#### src/navigation/index.js 생성

```javascript
import { createAppContainer, createSwitchNavigator } from 'react-navigation';
import { createStackNavigator } from 'react-navigation-stack';
import LoginScreen from './LoginScreen';
import HomeScreen from './HomeScreen';

const AuthStack = createStackNavigator(
    {
        LoginScreen: {screen: LoginScreen},
        HomeScreen: {screen: HomeScreen}
    },
    {
        initialRouteName: 'LoginScreen'
    }
);

// 최상단 네비게이터
const AppNavigator = createSwitchNavigator(
    {
        Auth: AuthStack
    },
    {
        initialRouteName: 'Auth',
    }
);

export default createAppContainer(AppNavigator);
```

 

기본 세팅이 끝났고 LoginScreen화면과 HomeScreen화면 간 이동하는 예제를 작성해보겠습니다.

 

#### LoginScreen.js 생성

```javascript
import React, { Component } from 'react';
import { View, Text, TouchableOpacity, StyleSheet, StatusBar } from 'react-native';
import { NavigationService } from '../../common';

class LoginScreen extends Component {
    constructor(props) {
        super(props);
    }
    render() {
        return (
            <View style={styles.container}>
                <StatusBar barStyle="dark-content" />
                <View>
                    <Text style={{fontSize:25}}>Login Screen</Text>
                </View>
                <TouchableOpacity
                        onPress={()=> NavigationService.navigate('HomeScreen', {
                            screen: 'HomeScreen',
                            info: 'information'
                        })}
                        style={{
                            justifyContent: 'flex-end',
                            backgroundColor: 'rgb(87,174,198)',
                            padding: 20,
                            marginTop: 20,
                            borderRadius: 30
                        }}>
                        <Text style={{fontSize: 20, textAlign: 'center', color: 'white'}}>다음</Text>
                    </TouchableOpacity>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: 'white',
        justifyContent: 'center',
    }
})

export default LoginScreen;  
```

 

#### HomeScreen.js 생성

```javascript
import React, { Component } from 'react';
import { View, Text, TouchableOpacity, StyleSheet, StatusBar } from 'react-native';
import {NavigationService} from '../../common';

class HomeScreen extends Component {
    constructor(props) {
        super(props);
    }
    render() {
        return (
            <View style={styles.container}>
                <StatusBar barStyle="dark-content" />
                <View>
                    <Text style={{fontSize:25}}>Home Screen</Text>
                </View>
                <TouchableOpacity
                        onPress={()=> NavigationService.back()}
                        style={{
                            justifyContent: 'flex-end',
                            backgroundColor: 'rgb(87,174,198)',
                            padding: 20,
                            marginTop: 20,
                            borderRadius: 30
                        }}>
                        <Text style={{fontSize: 20, textAlign: 'center', color: 'white'}}>뒤로</Text>
                    </TouchableOpacity>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: 'white',
        justifyContent: 'center',
    }
})

export default HomeScreen;  
```