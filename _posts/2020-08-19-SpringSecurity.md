# SpringSecurity



## 목차

______

- [보안 관련 용어](#보안 관련 용어)

1. [Spring Security란?](#1.-Spring-Security란?)

2. [Spring Security의 구조](#2.-Spring-Security의-구조)

   2-1. [인증관련 Architecture](#2-1.-인증관련-architecture)
   
   2-2. [Security의 Filter들](#2-2.-security의-filter들)
   
   2-3. [AuthenticationManager](#2-3.-authenticationmanager)
   
   2-4. [AuthenticationManager](#2-4.-authenticationmanager)
   
3. [설정하기](#3.-설정하기)

   3-1. [의존성 추가](#3-1.-의존성-추가)



## 보안 관련 용어

______

- 접근 주체(Principal)

  보호된 리소스에 접근하는 대상

- 인증(Authentication)

  어플리케이션의 작업을 수행할 수 있는 주체인지 확인

- 인가(Authorize)

  리소스에 접근 가능한 권한을 가졌는지 확인(after Authentication)

- 권한

  인증된 주체가 어플리케이션의 동작을 수행할 수 있도록 허락되있는지를 결정

  - 주체 증명 필요
  - 권한 부여에는 두가지 영역 존재
    1. 웹 요청 권한
    2. 메소드 호출 및 도메인 인스턴스에 대한 접근 권한



## 1. Spring Security란?

__________

스프링 기반의 어플리케이션 보안(인증과 권한)을 담당하는 스프링 하위 프레임워크입니다.

- Filter 기반으로 동작
  - MVC와 분리되어 관리 및 동작
- security 3.2 이상부터 XML로 설정하지 않고도 자바 bean 설정으로 간단하게 설정할 수 있도록 지원



## 2. Spring Security의 구조

______

### 2-1. 인증관련 Architecture

![](https://user-images.githubusercontent.com/41600558/90468752-24408f00-e152-11ea-8bac-8911025bc0f6.png)

위 그림은 Form 기반 로그인에 대한 플로우를 보여주는 그림입니다.

1. Form 로그인 시도한다(Http Request)

2. AuthenticationFilter가 HttpServlet Request에서 사용자가 보낸 내용(아이디, 패스워드 등)을 인터셉트한다. 이를 인증용 객체(UsernamePasswordAuthenticationToken)로 만들어 AuthenticationManager 인터페이스(구현체 : ProviderManager)에게 위임한다.

3. 인증용 객체(UsernamePasswordAuthenticationToken를 전달받는다.

4. 실제 인증을 할 AuthenticationProvider에게 인증 객체를 전달한다.

5. UserDetailsService 객체에게 사용자 아이디를 넘겨주면 객체는 DB에서 사용자 정보를 UserDetails의 형태로 가져온다. 

   (인증용, 도메인 객체를 분리하지 않기 위해 실제 사용되는 도메인 객체에 UserDetails를 상속하기도 한다)

6. AuthenticationProvider는 실제 사용자의 입력정보와 UserDetails 객체를 가지고 인증을 시도한다.

7. 8. 9. 10.  인증이 완료되면 Authentication 객체를 **SecurityContextHolder**에 담은 이후 AuthenticationSuccessHandle을 실행한다.

             (인증 실패시 AuthenticationFailureHandler를 실행한다)

* **SecurityContextHolder** : Spring Security의 인메모리 세션저장소

### 2-2. Security의 Filter들

![](https://user-images.githubusercontent.com/41600558/90584668-15b7ad80-e20e-11ea-88dc-5b8c089fb605.png)

1. **SecurityContextPersistenceFilter**

   SecurityContextRepository에서 SecurityContext를 가져오거나 저장하는 역할

2. **LogoutFilter**

   로그아웃 URL로 오는 요청을 감시 및 로그아웃 처리

3. **(UsernamePassword) AuthenticationFiler** (아이디와 비밀번호를 사용하는 form 기반 인증)

   설정된 로그인 URL로 오는 요청을 감시하며, 유저 인증 처리

   1. AuthenticationManger를 통한 인증 실행
   2. 인증 성공 시, 얻은 AUthentication 객체를 SecurityContext에 저장 후 AuthenticationSuccessHandler 실행
   3. 인증 실패 시, AuthenticationFailureHandler 실행

4. **DefaultLoginPageGeneratingFilter**

   인증을 위한 로그인폼 URL을 감시

5. **BasicAuthenticationFilter**

   HTTP 기본 인증 헤더를 감시하여 처리

6. **RequestCacheAwareFilter**

   로그인 성공 후, 원래 요청 정보를 재구성하기 위해 사용

7. **SecurityContextHolderAwareRequestFilter**

   HttpServletRequestWrapper를 상속한 SecurityContextHolderAwareRequestWrapper 클래스로 HttpServletRequest 정보를 감싼다

   SecurityContextHolderAwareRequestWrapper 클래스는 필터 체인상의 다음 필터들에게 부가정보를 제공

8. **AnonymousAuthenticationFilter**

   이 필터가 호출되는 시점까지 사용자 정보가 인증되지 않았다면 인증토큰에 사용자가 익명 사용자로 나타난다

9. **SessionManagementFilter**

   인증된 사용자와 관련된 모든 세션을 추적

10. **ExceptionTranslationFilter**

    보호된 요청을 처리하는 중에 발생할 수 있는 예외를 위임하거나 전달하는 역할

11. **FilterSecurityInterceptor**

    AccessDecisionManager로 권한부여 처리를 위임함으로써 접근 제어 결정을 쉽게 해준다

### 2-3. AuthenticationManager

​	모든 접근 주체(=유저)는 Authentication 을 생성한다. 이는 SecurityContext에 보관되고 사용된다.

​	즉, Security의 세션들은 내부 메모리(SecurityContextHolder)에 쌓고 꺼내쓰는 것이다.

```java
// Authentication 인터페이스

public interface Authentication extends Principal, Serializable { 
    Collection<? extends GrantedAuthority> getAuthorities(); // Authentication 저장소에 의해 인증된 사용자의 권한 목록 
    Object getCredentials(); // 주로 비밀번호 
    Object getDetails(); // 사용자 상세정보 
    Object getPrincipal(); // 주로 ID 
    boolean isAuthenticated(); // 인증 여부 
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException; 
}
```



### 2-4. AuthenticationManager

![](https://user-images.githubusercontent.com/41600558/90594167-b7e29000-e224-11ea-9643-570b2e5c2897.png)

 유저의 요청 내에 담긴 Authentication를 AuthenticationManager에 넘기고 ProviderManager가 처리한다. ProviderManager는 정확히 `private List<AuthenticationProvider> providers;`로 여러 AuthenticationProvider가 처리하여 Authentication을 반환한다.

- AuthenticationManager : 인증요청을 받고 Authentication 을 채운다.
- AuthenticationProvider : 실제 인증이 일어나며, 성공하면 Authentication.isAuthenticated = true 를 한다.



## 3. 설정하기

________

### 3-1. 의존성 추가

```xml
<!-- Properties --> 
<security.version>4.2.7.RELEASE</security.version> 

<!-- Security --> 
<dependency> 
    <groupId>org.springframework.security</groupId> 
    <artifactId>spring-security-core</artifactId> 
    <version>${security.version}</version> 
</dependency> 
<dependency> 
    <groupId>org.springframework.security</groupId> 
    <artifactId>spring-security-web</artifactId> 
    <version>${security.version}</version> 
</dependency> 
<dependency> 
    <groupId>org.springframework.security</groupId> 
    <artifactId>spring-security-config</artifactId> 
    <version>${security.version}</version> 
</dependency> 
<dependency> 
    <groupId>org.springframework.security</groupId> 
    <artifactId>spring-security-taglibs</artifactId> 
    <version>${security.version}</version> 
</dependency> 
<dependency> 
    <groupId>org.springframework.security</groupId> 
    <artifactId>spring-security-test</artifactId> 
    <version>${security.version}</version> 
</dependency>
```

