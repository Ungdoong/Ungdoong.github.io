# 190718(Thu)_Java

### 컴파일 및 실행 과정

- 이클립스는 실시간 컴파일
- HelloTest.java →
  - Compile : javac HelloTest.java
- HelloTest.class(ByteCode) →
  - Execute : java HelloTest
- JVM → AnyOS

### Java Keyword

- Java 문법에서 사용 용도가 정의된 예약어

### 식별자 명명 규칙

- class
  - 첫글자를 대문자로, 나머지는 소문자
- method
  - 모두 소문자
- variable
  - 모두 소문자
- constant(primitive)
  - 모두 대문자

### 서로 다른 타입의 연산

- 타입의 사이즈가 큰 형식을 따름

  ```
  int x = 1;
  long y = 2;
  print(type(x*y));
  
  >>long
  ```

  

### 참조형 변수

- class, interface, 배열 등

### 연산자

- ^(정수형) : 비트 XOR
- ^(논리형) : 논리 XOR
- ?: (논리형, 모든 데이터형) : 삼항

### 배열

- 배열은 생성만 해도 타입별로 default 값이 들어가 있음

### Method(함수)

- 일, 동작, 행위, 기능 등

- `수식어`_ `리턴타입`_ `식별자`(`매개변수1`. `매개변수2`, ...){

  }

