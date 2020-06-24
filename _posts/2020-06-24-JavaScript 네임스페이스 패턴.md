# JavaScript: 네임스페이스 패턴



전역변수를 기초로 하는 JavaScript의 단점에 의해, 전역 네임스페이스(Global Namespace)의 오염 문제는 신경써야할 부분입니다. 전역 변수가 증가할수록 변수명에 충돌이 발생할 우려가 있습니다. 또한 접근이 용이하여 코드의 신뢰성이 저하될 수 있습니다. 이러한 단점을 보완하는 몇 가지 패턴에 대하여 살펴보겠습니다.

1. [var](#1.-var)

2. [즉시 실행 함수 & 즉시 객체 초기화](#2.-즉시-실행-함수-&-즉시-객체-초기화)

   - [즉시 실행 함수](#즉시-실행-함수(iife,-immediately-invoked-function-expression))
   - [즉시 객체 초기화](#즉시-객체-초기화)

3. [네임스페이스 패턴](#3.-네임스페이스-패턴(namespace-pattern))

   - [객체 리터럴 네임스페이싱](#객체-리터럴-네임스페이싱(object-literal-namespacing))
   - [범용 네임스페이스 함수](#범용-네임스페이스-함수)
   - [샌드박스 패턴](#샌드박스-패턴(sandbox-pattern))

   [참고 자료](#참고-자료)



## 1. var

_________

JavaScript에는 암묵적 전역(implied globals)이라는 개념이 있습니다. 때문에 아래의 경우에는 지역 함수 내에 있더라도 '전역에 속하게' 됩니다.

- var를 사용하지 않고 변수 선언
- 선언되지 않은 변수를 사용

'전역에 속한다'라고 표현한 이유는 var를 사용하지 않았을 경우에는 전역 변수가 아닌 **전역 객체의 프로퍼티(property)**로 생성되기 때문입니다.

```javascript
/*
 * a : 함수에 속하지 않고 var를 사용하여 선언된 전역 변수
 * b : 함수에 속하지 않고 암묵적으로 생성된 전역 객체의 프로퍼티
 * c : 함수에 속하고 암묵적으로 생성된 전역 객체의 프로퍼티
 */

var a = 1;  
b = 2;  
(function () { 
    c = 3; 
}());

delete a;  
delete b;  
delete c;

console.log(typeof a);        // number 출력  
console.log(typeof b);        // undefined 출력  
console.log(typeof c);        // undefined 출력  
```

프로퍼티는 delete연산자로 삭제할 수 있지만 변수는 그렇지 않다는 특성을 통해, 변수 b와 c는 전역 변수가 아닌 전역 객체의 프로퍼티가 됨을 알 수 있습니다.

### 단일 var 패턴

 var 선언을 하나만 쓰고 **여러 개의 변수를 쉼표로 연결하여 선언**합니다. 이는 **모든 지역변수를 한 곳에서 찾을** 수 있게 해주고, 변수를 선언하기 전에 사용하면 발생하는 **논리적인 오류를 막아**준다는 장점이 있습니다.

But, 복잡한 소스 코드의 구조 안에서 모든 변수를 함수의 위쪽에 선언한다는 것은 어려우므로, 이 패턴은 모듈 단위 개발에서 효과가 있습니다.



## 2. 즉시 실행 함수 & 즉시 객체 초기화

________

 전역 변수를 감소시키기 위하여 즉시 실행 함수와 즉시 객체 초기화라는 패턴이 있습니다.

### 즉시 실행 함수(IIFE, Immediately-Invoked Function Expression)

 **선언과 동시에 바로 실행**되므로 전역 변수를 생산하지 않습니다. **플러그인**이나 **라이브러리** 등을 만들 때 사용되며 **모듈 패턴의 기반**이 됩니다.

```javascript
console.log(typeof  
// 함수 선언문
function func() {  
    var days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    var today = new Date();
    var msg = 'Today is ' + days[today.getDay()] + ', ' + today.getDate(); 
    console.log(msg);  
}
);   // function 출력
```

전역 객체에 func라는 이름의 함수가 추가됨을 알 수 있습니다.

```javascript
console.log(typeof  
// 즉시 실행 함수
(function () {
    var days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    var today = new Date();
    var msg = 'Today is ' + days[today.getDay()] + ', ' + today.getDate(); 
    console.log(msg);  // Today is Mon, 24 출력
}())
);   // undefined 출력
```

즉시 실행 함수 표현식으로 선언된 코드는 **함수가 즉시 실행되고 난 후 전역에 남지 않고 바로 사라짐**을 알 수 있습니다. (기명 함수로 작성하였을 경우에도 바로 사라짐)

그러므로, **한번만 호출한 후 재사용할 필요가 없는 함수**들은 즉시 실행 함수 패턴을 사용하여 전역 네임스페이스의 오염을 줄일 수 있습니다.



### 즉시 객체 초기화

 괄호로 묶어서 바로 초기화하는 사용 방식과 전역 변수를 만들지 않는다는 장점이 즉시 실행 함수와 비슷합니다. 그러나 대부분의 JavaScript 압축 도구가 즉시 실행 함수에 비해 효과적으로 압축하지 못한다는 단점이 있습니다.

```javascript
// 즉시 객체 초기화 패턴
({
    // 속성 정의
    name: "ungdoong",

    // 객체 메소드 정의
    getName: function () {
        return this.name;
    },

    // 초기화 메소드 정의
    init: function () {
        console.log(this.getName());   // ungdoong 출력
    }
}).init();
```



즉시 실행 함수와 즉시 객체 초기화 사용 시 주의할 점은 **메모리 낭비**입니다. JavaScript는 위와 같은 패턴처럼 **할당 없는 정의**의 경우, 전역 네임스페이스는 건드리지 않더라도 **전역 실행 컨텍스트(EC: Execution Context)의 temp**라는 리스트에 key-value를 추가하게 됩니다. 이 EC.temp 영역은 **개발자가 접근할 수 없는 영역**입니다.  접근이 불가하므로 코드의 신뢰성에는 도움이 되지만, 위의 패턴을 남용하면 **관리할 수 없는 메모리 공간에 누적**됩니다. 그러므로 코드의 **신뢰성과 메모리 사이에서 고민**이 요구됩니다.



## 3. 네임스페이스 패턴(Namespace Pattern)

_______

 네임스페이스(Namespace)는 구분이 가능하도록 정해놓은 범위나 영역을 말합니다. 즉, 이름 공간을 선언하여 **다른 공간과 구분**하도록 합니다.

 네임스페이싱을 하는 이유는 **객체나 변수명이 겹치지 않도록** 하여 코드의 안전성을 높이기 위함입니다. JavaScript는 네임스페이싱을 위한 기능을 지원하지 않기 때문에 다음의 특성을 이용하여 비슷한 효과를 얻을 수 있습니다.

- JavaScript의 모든 객체는 프로퍼티를 가짐
- 그 프로퍼티는 다시 다른 객체를 담을 수 있음

### 객체 리터럴 네임스페이싱(Object Literal NameSpacing)

 가장 기본적인 네임스페이스 패턴으로써 **하나의 전역 객체(global object)를 만들어 이곳에 모든 함수, 객체, 변수를 추가**하여 구현합니다. 전역 객체 시에 리터럴로 미리 선언하거나 나중에 추가할 수 있습니다.

```javascript
// 하나의 전역 객체
var MYAPP = {};

MYAPP.Parent = function() { console.log('Parent'); };  
MYAPP.Child = function() { console.log('Child'); };

MYAPP.variable = 1;

// 객체 컨테이너
MYAPP.modules = {};

// 객체들을 컨테이너 안에 추가합니다.
MYAPP.modules.module1 = {};  
MYAPP.modules.module1.data = {a: 1, b: 2};  
MYAPP.modules.module2 = {};

MYAPP.Parent();                               // Parent 출력  
console.log(MYAPP.modules.module1.data.a);    // 1 출력  
MYAPP.Child();                                // Child 출력 
```

 이 패턴은 **코드 내에서 뿐만아니라 같은 페이지에 존재하는 JS 라이브러리나 서드 파티 코드(third-party code)와의 이름 충돌도 방지**해주며 체계적이라는 장점이 있습니다.

But, 아래와 같은 단점도 존재합니다.

- 상위 객체명을 붙여야하므로 코드량이 증가함

- 객체의 한 부분만 수정되어도 해당 객체 인스턴스 전체를 갱신함

- 매번 객체에 접근하며, 이름이 중첩되고 길어지므로 검색속도가 저하됨

  → 전역객체로의 접근없이 바로 상위 객체로 접근하는 this키워드의 사용을 생각할 수 있으나, **네임스페이스로 사용되고 있는 객체는 this를 사용하여 참조해서는 안됨**

  ```javascript
  var MYAPP = {};
  
  MYAPP.message = "Hi";  
  MYAPP.sayHello = function() {  
      // 네임스페이스 명을 사용하여 리턴
      return MYAPP.message;
  };
  
  console.log(MYAPP.sayHello());    // Hi 출력  
  var direct = MYAPP.sayHello;  
  console.log(direct());            // Hi 출력  
  var MYAPP = {};
  
  MYAPP.message = "Hi";  
  MYAPP.sayHello = function() {  
      // this를 사용하여 리턴
      return this.message;
  };
  
  console.log(MYAPP.sayHello());    // Hi 출력  
  var direct = MYAPP.sayHello;  
  console.log(direct());            // undefined 출력  
  ```

   아래쪽 코드에서 네임스페이스명을 사용하여 출력하였을 때는 원하는 결과를 리턴하지만, 새로운 전역변수에 대입할 경우 undefined를 출력합니다. 이는 함수 영역 안에 있는 **this키워드는 부모의 자식으로 불렸을 때만 부모객체를 가리키고, 직접 호출하였을 때는 부모객체가 아닌 전역 객체를 가리키기 때문**입니다.

   그러므로, this를 네임스페이스로 사용되는 객체에 사용할 경우 네임스페이스로부터 식별자(identifier)를 가져오는데 혼란을 야기합니다.

   이러한 단점을 해결하기 위해 this키워드가 아닌 샌드박스 패턴(Sandbox Pattern)을 사용합니다. 이는 뒤에서 살펴보겠습니다.

### 범용 네임스페이스 함수

 프로그램의 복잡도에 따라 코드의 각 부분들이 별개의 파일로 분리되어 선택적으로 문서에 포함되는 경우가 있습니다. 이러한 상황에서 네임스페이스로 사용할 객체를 선언하거나, 그 내부의 프로퍼티를 정의할 때, **의도치않은 오버라이딩을 시도하는 문제**가 있습니다.

```javascript
// 1번
var MYAPP = {};

// 2번
if (typeof MYAPP === "undefined") {  
    var MYAPP = {};
}

// 3번
var MYAPP = MYAPP || {};  
```

 따라서 1번과 같은 선언보다는 2번처럼 **해당 객체명이 미리 선언되었는지를 확인하고 정의하는 것이 바람직**합니다. 3번은 2번과 똑같이 동작하는 short-hand 방식입니다. 

 But, 이는 네임스페이스의 깊이가 깊어질수록 **많은 중복코드를 양산**할 수 있습니다. 그러므로 아래와 같이 재사용 가능한 함수를 작성하는 것이 좋습니다.

```javascript
// 가장 상단에 위치할 객체는 먼저 선언합니다. 
// (namespace함수를 전역으로 선언하지 않기 위함입니다.)
var MYAPP = MYAPP || {};

MYAPP.nsFunc = function (ns_string) {  
    // '.'으로 구분된 네임스페이스 표기를 쪼갭니다
    var sections = ns_string.split('.'),
        parent = MYAPP,
        i;

    // 최상단의 MYAPP객체는 이미 선언되었으므로 제거합니다.
    if (sections[0] === "MYAPP") {
        sections = sections.slice(1);
    }

    var s_length = sections.length;
    for (i=0; i<s_length; i+=1) {
        // 프로퍼티가 존재하지 않아야만 생성합니다.
        if (typeof parent[sections[i]] === "undefined") {
            parent[sections[i]] = {};
        }
        parent = parent[sections[i]];
    }
    return parent;
};
```

 생성한 namespace 함수를 이용해 다음과 같은 긴 네임스페이스도 안전하게 만들 수 있습니다.

```javascript
MYAPP.nsFunc('korea.seoul.geumcheongu.gasandigital1ro.jeiplatz.name');  
console.log(window.MYAPP);  
```

![](https://user-images.githubusercontent.com/41600558/85550461-f3654180-b65b-11ea-886a-3c2ccf9c5a7f.png)

### 

### 샌드박스 패턴(Sandbox Pattern)

 샌드박스는 어린이가 안에서 노는 모래놀이 통을 의미합니다. 모래를 담은 상자 밖으로 모래를 흘리거나 더럽히지 않기 위한다는 의미가 담겨있습니다.  마찬가지로 코드들이 **전역 범위를 더럽히지 않는 선에서 마음껏 쓰일 수 있도록 유효범위를 지정**해줍니다.

 위에서 살펴본 네임스페이스 패턴에서는 단 하나의 전역 객체를 생성했습니다. 샌드박스 패턴에서는 **생성자를 유일한 전역으로 사용**합니다. 그리고 유일한 전역인 생성자에게 **콜백 함수(Callback function)를 전달**해 모든 기능을 샌드박스 내부 환경으로 격리 시키는 방법을 사용합니다.

```javascript
function Sandbox() {  
        // argument를 배열로 바꿉니다.
    var args = Array.prototype.slice.call(arguments),
        // 마지막 인자는 콜백 함수 
        callback = args.pop(),
        // 모듈은 배열로 전달될 수도있고 개별 인자로 전달 될 수도 있습니다.
        modules = (args[0] && typeof args[0] === "string") ? args : args[0],
        i;

    // 함수가 생성자로 호출되도록 보장(new를 강제하지 않는 패턴)
    if (!(this instanceof Sandbox)) {
        return new Sandbox(modules, callback);
    }

    // this에 필요한 프로퍼티들을 추가
    this.a = 1;
    this.b = 2;

    // "this객체에 모듈을 추가"
    // 모듈이 없거나 "*"(전부)이면 사용 가능한 모든 모듈을 사용한다는 의미입니다.
    if (!modules || modules === '*' || modules[0] === '*') {
        modules = [];
        for (i in Sandbox.Modules) {
            if (Sandbox.modules.hasOwnProperty(i)) {
                modules.push(i);
            }
        }
    }

    // 필요한 모듈들을 초기화
    var m_length = modules.length;
    for (i=0; i<m_length; i+=1) {
        Sandbox.modules[modules[i]](this);
    }

    // 콜백 함수 호출
    callback(this);
}

// 필요한 프로토타입 프로퍼티들을 추가
Sandbox.prototype = {  
    name: "ungdoong",
    getName: function () {
        return this.name;
    }
};
```

그리고 다음과 같이 new키워드 대신 'ajax'와 'dom'이라는 가상의 모듈을 사용하는 객체를 생성합니다. 이처럼 사용할 모듈들을 앞쪽 인자(argument)로 전달해주고, 마지막 인자로는 콜백 함수를 전달해주면 됩니다.

```javascript
Sandbox ('ajax', 'dom', function (box) {  
    console.log(box);
});
```

이렇게 샌드박스 패턴을 사용하면 콜백 함수로 감싸진 샌드박스 객체 안에서 마음껏 구현이 가능합니다. 더불어 위에어 언급한 네임스페이스 패턴의 몇 가지 단점을 극복할 수 있습니다.

- 단 하나의 전역변수에 의존하는 네임스페이스 패턴의 단점을 **여러 개의 샌드박스 객체를 생성함으로 극복**
- 점으로 연결된 긴 이름을 쓸 필요가 없음
- 런타임(Runtime)에 탐색 작업을 거치지 않게 해줌



## 참고 자료

______

- nextree님 블로그 - http://www.nextree.co.kr/p7650/