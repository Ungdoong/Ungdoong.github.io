# 190725(Thu)_Java\_문법

### 추상 메소드

- 함수의 body({}) 부분이 없는 미완성의 함수

  ```java
  public abstract void go(String m);
  ```

  

### 추상 클래스

- 추상 메소드를 가지는 클래스

  ```java
  public abstract class MyAbstract{
  
  	public abstract void go(String m);
  
  }
  ```

- 미완성의 클래스 이므로 생성불가
  - MyAbstract m = new MyAbstract(); ( X )
- 다른 클래스에서 추상 클래스를 상속해서 미완성의 메소드를 구현(완성)해 줘야 함

### 

### 인터페이스

- 클래스 안의 모든 메소드가 추상메소드인 미완성 클래스

  ```java
  interface MyInterface{
  	public void go();
  }
  MyInterface m = new MyInterface(); (X)
  ```

- 다중 상속을 지원하기 위해 나옴



### 접근 지정자

- 자식이 부모의 메소드를 오버라이딩 할 때, 자식메소드 접근 지정자의 범위는 부모의 메소드보다 크거나 같아야 한다.