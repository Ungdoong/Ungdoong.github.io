# 191023(Web)_Spring+Mybatis

### ORM

- Object(자바객체)
- Relation(table)
- Mapping

### DI(Dependency Insection)

- 의존성이 생기면 객체를 주입
- 주입방식
  - Constructor Insection(생성자 주입)
  - Setter Insection(Setter 주입)

### MVNRepository

- pom.xml에 추가해서 사용

### AspectJ

### AOP(Aspect Oriented Programming)

- 어떤 로직을 기준으로 핵심기능, 부가기능으로 나누고 각각 모듈화

### TransactionManager

- 트랜젝션 매니저 이름을 transactionManager인 기본 이름으로 지정 하지 않을 경우 tx 생성시에 지정해 주어야 함

- 기본이름이면 지정하지 않아도 됨

  ```xml
  <tx:annotation-driven transaction-manager="<트랜잭션매니저 이름>"/>
  ```

### 어노테이션

- 클래스에 붙이는 어노테이션
  - @Component
  - @Service
  - @Repository
  - @Controller
- 필드에 붙이는 어노테이션
  - @Autowired
- 메소드에 붙이는 어노테이션
  - @Transactional