# 190723(Tue)_Java\_문자열

## String 클래스

### StringBuilder

- String 변수의 값을 변경하는것은 추천하지 않음
- 변경이 필요할 때는 StringBuilder 사용
- equals를 오버라이딩 하지 않아 사용못함
  - StringBuilder끼리는 비교불가
  - .toString()으로 String 타입으로 변경 후 비교

### reverse

- String 클래스는 reverse 메소드가 없음

```java
String s="ABC";
StringBuilder sb = new StringBuilder(s);
System.out.println(sb.reverse());
```



### StringBuffer & StringBuilder

- StringBuffer는 Syncronized 존재
- StringBuilder가 더 빠름
- StringBuilder는 Single Thread일 경우만 사용

### String to int & int to String

- C언어
  - atoi() or itoa()
- java
  - Integer.parseInt(String)
  - Int.toString()