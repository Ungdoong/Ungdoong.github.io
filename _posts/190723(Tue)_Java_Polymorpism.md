# 190723(Tue)_Java\_Polymorpism

### Polymorphism Definition(다형성)

1. Polymorphism .,,,,상속!

```java 
int i='A'; // int 타입에 string 가능 
Object jj=new int 456; //???
long l=123l; //64bit
float f=123l; //32/64bit 초과된 메모리는 버려짐
// 자동 형변환 
// 형변환하면 사용가능, 하는 법은
if(p instanceof Student){
    Student s=(Student)p;
    s.setStuid(20190205);
}

Person p=new Student(); 
// student는 person을 상속해 메모리가 더크지만 person이 더 큰집합이다!
p.setName("잰");
p.setStuid(1213);//err
Syso(p.toString()); //ok child
```

2. polymorphic array

```java
int[]ia=new int[3];
ia[0]='A';
//object배열에는 다들어감, 다차원 배열을 만들 필요가 없음
Person[] pa=new Person[3];
pa[0]=new Student();
```

3. polymorphic parameter

```java
void set(int i) {}
set(new Person());//호출가능
set('A');
```

4. polymorphic return

```java
int get(){
        //...
    return 'A';
    return new Person(); //가능
}
```

5. polymorphic overide

```java
#include<stdio.h>

int main(){
    printf("HelloC\n"); //-> stdio.h 에 있다, 헤더의 원형엔 구현부X 
                      //runtime시점에 오버라이딩된 메소드가 호출되어 구현되어지는 것!
                       //헤더의 집합_ 라이브러리(인터페이스의 집합_ API)
    return 0;
}
```