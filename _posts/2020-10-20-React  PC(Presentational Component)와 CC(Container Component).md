**PC(Presentational Component)**와 **CC(Container Component)**의 차이를 알아보도록 하겠습니다.

React에서 Component는 상태관리, DOM 관리, 이벤트 관리 등 다양한 역할을 하는 중요한 개념입니다. 하지만, 다양한 작업들을 모든 컴포넌트마다 처리하는 것은 소스를 복잡하고 가독성이 떨어지게 만들며, 결론적으로 유지보수가 힘들어지게 됩니다. 이를 보완하기 위해 PC와 CC의 개념을 사용합니다.

결론적으로, PC와 CC의 개념을 사용하는 목적은 재사용성과 유지보수성을 높이기 위함입니다. 각각의 개념과 성질, 패턴 사용법을 살펴보겠습니다. 이 포스트에서는 Redux 체계에서의 개념 설명이 주를 이룹니다.



## 개념

### Presentational Component

어떻게 보여지는 지를 책임. 데이터를 불러오거나 변경하지 않고 부모로부터 props를 통해 데이터를 전달받아 사용

### Container Component

어떻게 동작해야 할지를 책임. 데이터 및 데이터와 관련된 동작을 다른 PC와 CC에게 제공



## 성질

### Presentational Component

- 내부에 PC와 CC 모두 가질 수 있고, 대게 DOM Mark-up과 자체 스타일을 가짐
- Styled Components는 모두 PC이다
- 데이터나 상태에 관해 관여하지 않아 결합도가 낮고 재사용성이 높음
- state, LifeCyle, 성능 최적화가 필요없는 경우라면 Functional Component로 작성
- 예) Page, Slidebar, Story, UserInfo, List

### Container Component

- 내부에 PC와 CC 모두 가질 수 있고, 대게 전체를 감싸는 div를 제외한 자체적인 DOM 마크업이나 스타일은 가지지 않음
- 데이터 및 데이터와 관련된 동작을 다른 PC와 CC에게 제공
- 직접 작성되기 보다는 HOC(Higher-Order Components)로부터 생성되는 경우가 많음
  - React Redux가 제공해주는 connect()함수를 사용해 container 컴포넌트 생성
- `mapStateToProps`, `mapDispatchToProps` : CC가 PC에게 Redux 안의 상태를 어떤 prop으로 내려줄지 정의
- 예) UserPage, FollowersSlidebar, StoryContainer, FollowedUserList



## 사용법

패턴의 사용법을 말하기에 앞서 두 컴포넌트에 대한 명확한 구분을 위해 서로 다른 폴더로 관리하는 것을 권장합니다.

### Container Component

`src/components/people/container/PeoInfoFrame.jsx`

```jsx
import React, { Component } from 'react';
import PeoList from '../presentational/PeoList';

class PeoInfoFrame extends Component {
  state = {
    id: 3,
    peopleList: [
      { id: 0, name: 'jerome', age: 28 },
      { id: 1, name: 'baek', age: 26 },
      { id: 2, name: 'yeob', age: 30 }
    ]
  };

  deletePeople = id => {
    const { peopleList } = this.state;
    this.setState({
      peopleList: peopleList.filter(info => info.id !== id)
    });
  };

  render() {
    const { peopleList } = this.state;
    return <PeoList info={peopleList} onRemove={this.deletePeople} />;
  }
}

export default PeoInfoFrame;
```

`PeoInfoFrame`은 DOM 마크업에서 `PeoList`만을 포함하며 다른 마크업 구성은 존재하지 않는다.

`peopleList`의 상태를 관리하고 이를 `PeoList` 컴포넌트에게 prop으로 전달하는 CC의 역할을 수행한다.



### Presentational Component

`src/components/people/presentational/PeoList.jsx`

```jsx
import React from 'react';
import PeoInfo from './PeoInfo';

const PeoList = ({ info, onRemove }) => {
  const list = info.map(peo => (
    <PeoInfo info={peo} onRemove={onRemove} key={peo.id} />
  ));
  return <div>{list}</div>;
};

export default PeoList;
```

`src/components/people/presentational/PeoInfo.jsx`

```jsx
import React from 'react';

const PeoInfo = ({ info, onRemove }) => {
  const style = {
    border: '1px solid black',
    padding: '8px',
    margin: '8px'
  };
  const { name, age, id } = info;
  return (
    <div style={style}>
      <div>
        <b>{name}</b>
      </div>
      <div>
        <b>{age}</b>
      </div>
      <button onClick={() => onRemove(id)}>삭제</button>
    </div>
  );
};

export default PeoInfo;
```

위의 두 컴포넌트는 상태관리에 관여하지 않으며 props로 전달된 데이터를 이용하여 디스플레이한다.



### 더하여

- Redux안의 상태를 `mapStateToProps`와 `mapDispatchToProps`를 이용하여 어떤 prop으로 전달할 지 정의할 수 있다.

- `mapStateToProps`와 `mapDispatchToProps`는 Redux 스토어의 상태가 변경될 때마다 호출되고, 이에 따라 PC 또는 CC에 새로운 props가 전달된다.
- props에 변경점이 생기면 해당 컴포넌트는 재렌더링을 수행한다.

Redux-React와 관련한 내용은 다음 포스트에서 다루겠습니다.



## 참조

- [https://velog.io/@smooth97/-Redux-Presentational-Component-Container-Component](https://velog.io/@smooth97/-Redux-Presentational-Component-Container-Component)
- [https://blog.naver.com/backsajang420/221368885149](https://blog.naver.com/backsajang420/221368885149)
- [https://velog.io/@jeonghoheo/Redux-React-%EC%9A%94%EC%95%BD](https://velog.io/@jeonghoheo/Redux-React-%EC%9A%94%EC%95%BD)