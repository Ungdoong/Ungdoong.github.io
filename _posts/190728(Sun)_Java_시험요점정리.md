# 190728(Sun)_Java\_시험요점정리

### 객체 지향의 특징

- Abstraction(추상화)
  - 현상에 존재하는 객체의 주요특징을 추출하는 과정
- Encapsulation(캡슐화)
  - 클래스 안의 중요한 데이터나 복잡한 기능 등은 숨기고, 외부에서 사용에 필요한 기능만을 공개
- Ingeritance(상속)
  - 객체 정의 시 기존에 존재하는 객체의 속성과 기능을 상속
- Polymorphism(다형성)
  - 같은 타입 또는 같은 기능의 호출로 다양한 효과를 가져오는 것

### 객체 지향 이점

- 재사용성의 증가 / 개발비 절감 / 품질 보증
- 더 많은 재사용성의 효과를 가져오기 위해 재사용단위가 큰 Component 기반의 개발 기법 등장 (CBD 발달)
- SW 아키텍처 발달, 필요한 라이브러리(API)를 추가한 Framework 개발 및 재사용

### 객체 지향 개념

- 개발자에게 쉽게 커스터마이징이 가능하게 함
- 유지보수성, 확장성, 재사용성을 증가
- CBD 기반의 개발은 재사용성과 교체성을 증가

### 용어 정리

- Framework
  - 기본적인 SW의 아키텍처 구현
  - 프로그래밍에 필요한 라이브러리 포함
  - 잘 짜인 구조가 미리 프로그래밍
    - 안정적이고 빠른개발 가능

### Java의 특징

- 플랫폼 독립적
  - Java로 작성된 응용프로그램은 JVM위에서 작동
    - 컴파일 코드가 플랫폼 의존이 아닌 JVM이 해석 가능하도록 번역
- 메모리 관리
  - Gabage Colletor
- 분산 프로그래밍 지원
  - 네트워크를 이용한 프로그래밍 지원 / 원격 접속을 위한 다양한 기술셋 보유
- 멀티 스레드
  - Thread API 제공
    - 운영체제에 종속적이지 않는 독립적인 설계
    - JVM에 으해 스케쥴링

![1564292780278](C:\Users\xorwl\AppData\Roaming\Typora\typora-user-images\1564292780278.png)

### 식별자

- class, method, variable
  - class  : 첫글자 대문자, 나머지 소문자
  - method : 모두 소문자
  - variable : 모두 소문자
  - constant : 모두 대문자
- 유니코드, 시작가능 특수문자( '_', '$' ), 두번째 글자부터 숫자가능
- java는 대소문자 구별, 길이 제한 x
- keyword 사용 x

### Primitive Type Casting

- Implicit Type Casting (Automatic promotions)
  - 작은 크기의 타입은 큰 크기의 타입으로 자동으로 형변환
- Explicit Type Casting
  - 큰 크기의 타입이 작은 크기의 타입으로 변경할 경우(타입명시)

- 여러 타입 연산시 가장 큰 타입으로 결과값을 갖는다.

- byte, short, char 타입의 이항연산 결과는 int

### 배열 복사

1. ```java
   System.arraycopy(array1, 0, array2, 0, array1.length);
   ```

   - 자바 네이티브 인터페이스를 사용 - 속도가 빠름
   - shallow clone
   - `int array1 = array2` 와 같은 결과이나 속도가 빠름

2. ```java
   array2 = Arrays.copyOf(array, array.lenght);
   ```

3. ```
   array2 = array1.clone();
   ```

   - 같은 내용을 가진 서로 다른 배열을 생성
   - deep clone

### 2차원 배열

- 행과 열이 각가의 객체로 생성되므로 행과 열의 생성을 분리 가능
  - 열의 길이가 각각 다른 2차원 배열 생성 가능

### Java 소스 문서화

```
>javadoc -d doc -private *.java
```

- 주석 속성
  - @see class_name : "See also"라는 항목을 만들어 해당 클래스와 link
  - @see clas_name#method_name : "See also"라는 항목을 만들어 특성 메서드와 link
  - @version text : HTML파일의 버전을 표시하는 항목을 만듬
  - @author text : HTML파일의 저자를 표시하는 항목을 만듬
  - @param name descripton : 특성 메서드가 취하는 파라미터를 기술할 때 사용
  - @return description : 특성 메서드의 리턴값을 기술할 때 사용
  - @exception class_name : 특성 메서드가 발생시킬 수 있는 예외사항을 기술할 때 사용
- 문서화 기능
  - 메뉴: Project → Generate Javadoc → 원하는 Project 선택 및 문서화 파일 위치 선택 → finish

### Java 메모리 관리

![1564297677679](C:\Users\xorwl\AppData\Roaming\Typora\typora-user-images\1564297677679.png)

- Class Loader : 하드에 있는 번역된 class 파일을 메로리로 로드

- Class Area(Method Area) : 메모리로 읽어온 클래스의 정보를 기억하는 곳

- Heap : 클래스의 객체를 생성하여 기억

- Stack 

  - 메서드 수행 시마다 프레임이 할당되어 메서드 수행에 필요한 변수, 중간 결과 값을 임시기억. 
  - 메서드 종료 시 할당 메모리 자동 제거

- Gabage Collection

  - Heap 영역(class 영역 포함)에 생성된 객체들의 메모리 관리 담당
  - 더 이상 사용되지 않는 객체들을 점검하여 제거

  - 자동적인 실행, CPU가 한가하거나 메모리가 부족할 때 JVM에 의해 실행

### 객체 생성 로직

1. stack에 변수 공간 할당(초기화 되지 않음)

2. Class Loader에게 클래스 로딩 요청

3. Class Loader는 클래스가 로딩되어 있지 않다면 로딩하고 레퍼런스 캐싱

   캐싱정보에 이미 존재한다면 로딩을 수행하지 않고 기존 레퍼런스 리턴

4. 로딩된 클래스 정보를 이용하여 객체 생성(Heap)

5. 클래스의 생성자를 수행

6. 생성된 객체의 레퍼런스를 변수에 저장

7. 다시 객체 생성을 호출하면 이미 클래스가 로딩되어 있으므로 객체만 생성

### 객체 메서드 수행

1. stack에 메서드를 수행할 공간(frame)을 할당하여 메서드 수행
2. 수행이 끝나면 할당된 메모리(frame) 자동 제거

### this

- 현재 실행중인 객체의 생성자 : this( args list)
  - 반드시 생성자의 첫라인에서만 사용

### Variable Life Cycle

- Member Variable
  - class 내에 정의되는 variable
  - 메모리 영역중 heap에 생성
  - static(class) variable
    - class가 메모리에 로딩시에 메모리 할당 및 자동 초기화
    - class 제거 시 제거됨
  - instance variable
    - 객체 생성시 자동으로 초기화
    - 객체 제거 시 제거
- Default 초기화
  - 정수형(byte, short, int, long) : 0
  - 실수형(float, double) : 0.0
  - boolean : false
  - char : \u0000
  - reference type : null
- Local Variable
  - 메모리 영역 중 stack에 생성
  - 자동으로 초기화 되지 않기 때문에 반드시 초기화 하여 사용

### Method Overloading

- 메서드명은 같고, 파라메터는 반드시 달라야 한다.
- 제한자, return type은 상관 없음

### Access Modifier(접근 제한자)

- public
  - class, package, subclass, universe
- protected
  - class, package, subclass
- default
  - class, package
- private
  - class

### Usage Modifier(사용 제한자)

- static
  - 객체 생성없이 사용하고자 할 경우 사용
- final
  - 변경할 수 없음
- abstract
  - 미완성

### Encapsulation

- private 사용 후 get, set 을 통해 접근, 제어

### Inheritance

- Generalization
- Specialization

- 상속되는 클래스의 속성이나 기능을 선택적으로 상속 X
- super의 생성자는 sub 클래스의 생성자내의 첫 라인에 super(..)로만 접근 가능
- 장점
  - 재사용성 증가
  - 일관성 증가
  - 유지보수성 증가
  - 명세서 역할
  - Polymorphism
- 단점
  - 지나친 상속은 프로그램 구조의 복잡성 증가
  - 지나친 상속은 JVM 관리에 부하
  - 지나친 상속은 성능 저하

### Method Overriding

- 식별자가 같음
- 리턴 타입은 일반적으로 같게 사용 (or 하위)
- 파라메터
- 접근 제한자가 같거나 넓은 범위
- 메서드 호출시 VM은 그 메서드가 Overriding 되어 있는지 여부 확인하고, 생성된 객체의 상속 관계에서 마지막 Overriding된 메서드를 호출

### Reference Type Casting

- Reference 자동(묵시적) 형변환

  ```
  SuperType var=sub객체;
  ```

  - 주의
    - super가 가지고 있는 member만 사용 가능
    - 실행 시 overriding 되어 있는 method가 있다면 overriding된 method를 수행
    - 서브 객체가 추가한 메서드는 사용할 수 없음

- Reference 명시적 형변환

  ```
  SubType var=(SubType)superType변수;
  ```

### 필요 시 자주 overrided 되는 메서드

- toString():String
- hashCode():int
- equals(Object o):boolean

### String

- 어떤 type이든 String 객체에 +를 하게 되면 문자열로 변환되어 결과값이 모두 String으로 변환

- StringBuilder
  - 문자열을 편집하여 사용하고자 할 경우에 사용

### Wrapper Class

Java의 기본형 데이터 타입을 객체화 시키는 클래스들

- 객체가 아닌 것 : primitive data type(계산을 위한 type)

  - byte, short, int ,long ,float, double, char, boolean
  - primitive type을 객체화 시켜주는 클래스

- primitive 객체화

  - ```java
    Integer iobject = new Integer(300);
    ```

  - ```java
    Integer iobject2 = 300; //java 5.0부터
    ```

- 객체를 primitive

  - ```java
    int ip = iobject.intValue();
    ```

  - ```java
    int ip2 = iobject; //java 5.0부터
    ```

- Wrrapper 클래스

  - byte - Byte
  - short - Short
  - int - Integer
  - long - Long
  - float - Float
  - double - Double
  - boolean - Boolean
  - char - Character

- Primitive ==> String

  ```java
  st = new Integer(a).toString();
     = String.valueOf(a);
     = a+"";
  ```

- Stirng ==> Object

  ```java
  Integer it = Integer.valueOf(st);
  Double db = Double.valueOf(st);
  ```

- String ==> Primitive

  ```java
  int i = Integer.parseInt(st);
  double d = Double.parseDouble("34.5");
  ```

### final

- final class : 상속 받을 수 없음
- final method : Override 할 수 없음
- final variable : 상수로 정의

### Interface

- 인터페이스는 객체 생성 X
- super type으로 사용 가능
- 상속된 sub class는 모든 메서드를 구현(Overriding)해야함

