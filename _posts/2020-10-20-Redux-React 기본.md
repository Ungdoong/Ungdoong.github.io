Redux-React는 상태관리를 위하여 React와 Redux를 연결해주는 역할을 합니다. Redux-React를 활용하여 하위 컴포넌트에게 prop을 전달하는 방법과 과정에 대해 알아보겠습니다.



## Provider

 하나의 컴포넌트로써, React로 작성된 컴포넌트들을 Provider 안에 넣으면 하위 컴포넌트들이 Provider를 통해 redux store로 접근이 가능해집니다.

- Provider를 이용하여 redux store에 연결

  `App.jsx`

  ```jsx
  import React from 'react'
  import ReactDOM from 'react-dom'
  import { Provider } from 'react-redux'
  
  import { App } from './App'
  import createStore from './createReduxStore'
  
  const store = createStore()
  
  ReactDOM.render(
    // React의 props처럼 redux로 만든 store를 Provider에적용한다.
    <Provider store={store}>
    // 이제 App 컴포넌트는 Store에 접근이 가능하다.
      <App />
    </Provider>,
    document.getElementById('root')
  )
  ```



## connect()

 Provider 컴포넌트 하위에 존재하는 컴포넌트에서 Store에 접근할 수 있도록 합니다.

- 사용예제

  ```jsx
  import { connect } from 'react-redux'
  import TodoApp from "./components/TodoApp"
   
   // connect 함수를 실행시키고 
   // TodoApp컴포넌트에서 store에 접근하게 만든다.
   const Todo = connect();
   export default Todo(TodoApp);
   
   // 위의 코드를 간단하게 만들면 아래와 같다.
   export default connect()(TodoApp);
  ```

  

## mapStateToProps

 `connect` 함수에 첫번째 인수로 들어가는 함수 혹은 객체입니다. `mapStateToProps`는 기본적으로 store가 업데이트 될 때마다 자동적으로 호출이 됩니다. 이를 원하지 않는다면 `null` 혹은 `undefined` 값을 제공해야 합니다.

- `mapStateToProps`의 첫번째 인자

  ```jsx
  // mapStateToProps는 기본적으로 state가 첫번째 인자로 사용된다.
  // 그로인해 우리는 state를 다룰수 있게된다.
  const mapStateToProps = state => ({ todos: state.todos })
  ```

- `mapStateToProps`의 두번째 인자

  ```jsx
  // mapStateToProps의 두번째 요소로는 우리가 원하는 객체를 인자로 넘겨주면된다.
  state와 ownProps모두 순수 객체여야 한다.
  const mapStateToProps = (state, ownProps) => ({
    todo: state.todos[ownProps.id]
  })
  ```

- `mapStateToProps`를 `connect`함수에 사용

  ```jsx
  import { connect } from 'react-redux'
  import TodoApp from "./components/TodoApp"
   
   const mapStateToProps = (state, ownProps) => ({
    todo: state.todos[ownProps.id]
    })
   
   //connect 첫번째 인자로 mapStateToProps 함수를 제공했다.
   export default connect(mapStateToProps)(TodoApp);
  ```



## mapDispatchToProps

 `connect`함수의 두번째 인자로 사용됩니다. store에 접근한 컴포넌트가 store의 상태를 업데이트 하기 위해 dispatch를 사용할 수 있게 만들어줍니다.

- `mapDispatchToProps`의 dispatch

  ```jsx
  /* 
  mapDispatchToProps는 첫번째 인자로
  redux의 dispatch를 사용한다.
  이를 통해 우리는 store의 상태를 변경할수있다.
  */
  const mapDispatchToProps = dispatch => {
    // 순수 객체를 반환해줘야한다.
    return {
      // 순수 action객체를 dispatch 해준다.
      increment: () => dispatch({ type: 'INCREMENT' }),
      decrement: () => dispatch({ type: 'DECREMENT' }),
      reset: () => dispatch({ type: 'RESET' })
    }
  }
  ```

- `mapDispatchToProps`로 dispatch 하기

  ```jsx
  import toggleTodo from "./actions/toggleTodo"
  
  const TodoApp = () => {
  	const { toggleTodo, todo } = this.props;
  	return (
      	<div>
          	// mapStateToProps로 반환한 todo에 접근할수있다.
          	{todo}
              /*
              mapDispatchToProps가 반환한 toggleTodo를 사용해
              button을 눌러 dispatch하게 만들어줄수있다.
              */
              <button onClick={() => toggleTodo()} />
          </div>
      )
  }
  const mapStateToProps = (state, ownProps) => ({
    todo: state.todos[ownProps.id]
  })
    
   const mapDispatchToProps = (dispatch) => {
    toggleTodo: action => dispatch(toggleTodo(action))
    }
    
   /*
   반드시 connect로 mapStateToProps, mapDispatchToProps를 넘겨주어야
   mapStateToProps와 mapDispatchToProps가 반환한 객체에 TodoApp컴포넌트가
   this.props로 접근할수있다.
   */
   export default connect(mapStateToProps, mapispatchToProps)(TodoApp);
  ```

  



## 참조

- [https://velog.io/@jeonghoheo/Redux-React-%EC%9A%94%EC%95%BD](https://velog.io/@jeonghoheo/Redux-React-%EC%9A%94%EC%95%BD)