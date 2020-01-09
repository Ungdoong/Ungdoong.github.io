# 190731(Wed)_Java\_이론

## Server

### VO

- 데이터를 가지고 있음

### DAO(Data Access Object)

- VO의 데이터를 이용 (CRUD)

- Create/Read/Update/Delete

## Client

### Main()이 포함된  Class - SE

### Web Browser & Mobile & etc - EE

##### 

## 문법

### 추상 클래스

- 생성을 막기 위해  class 앞에 abstract를 추가할 수 있음

  - ex) MouseAdaptor

    - 마우스로 클릭했을 때 해야하는 행동이 다르기 때문에 구현하지 않음

    - 상속하여 완성시켜 사용

    - 추상 메소드는 존재하지 않고 구현 메소드의 바디를 비워 놓음

       → 자식이 반드시 구현을 해야하는 제약이 사라짐

### 클래스가 final

- 상속 불가

- 내부 메소드도 final효과
- 메소드가 final이면 오버라이딩 불가

### static 변수

- 클래스 변수라고도 함
- Class Area에 Class가 로드될 때 한번 생성되어 객체간 값이 공유됨

### 인터페이스

- 인터페이스 안의 메소드가 기본적으로 가지는 수식어
  - abstract / publics

- 인터페이스 안의 필드가 기본적으로 가지는 수식어
  - final / static / public

### List / Set / Map

|          | List                          | Set                  | Map                        |
| -------- | ----------------------------- | -------------------- | -------------------------- |
| data순서 | O                             | X                    | X                          |
| 중복     | O                             | X                    | X                          |
|          | **Vector**<br />**ArrayList** | HashSet<br />TreeSet | **Hashtable**<br />HashMap |

