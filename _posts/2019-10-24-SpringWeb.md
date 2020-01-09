# 191024(Tus)_SpringWeb

### 프로젝트 생성

1. Spring Legacy Project

2. Spring Web Project

3. pom.xml 에 디펜던시 추가

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.17</version>
   </dependency>
   <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.2.8</version>
   </dependency>
   <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis-spring</artifactId>
       <version>1.1.1</version>
   </dependency>
   
   <dependency>
       <groupId>commons-dbcp</groupId>
       <artifactId>commons-dbcp</artifactId>
       <version>1.4</version>
   </dependency>
   
   <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjrt -->
   <dependency>
       <groupId>org.aspectj</groupId>
       <artifactId>aspectjrt</artifactId>
       <version>1.8.9</version>
   </dependency>
   
   <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
   <dependency>
       <groupId>org.aspectj</groupId>
       <artifactId>aspectjweaver</artifactId>
       <version>1.8.9</version>
   </dependency> 
   ```

4. src>main>webapp>WEB-INF>spring>appServlet에 db.xml, mybatis-db.xml 추가

5. servlet-context.xml에 beans 추가

   ```xml
   <beans:import resource="mybatis-db.xml"/>
   ```

### 프로젝트 생성시 체크할 것

- web.xml
  - <context-param>

### Model객체

- Map 구조의 데이터 저장 객체

### 루트명 변경

- 프로젝트의 Properties>WebProjectSettings에서 변경
- 변경된 것 확인은 Servers 프로젝트에서 server.xml의 <Context ~> 확인

### @RequestMapping

- Handler Mapping의 역할->어느 컨트롤러가 요청을 처리할지 결정