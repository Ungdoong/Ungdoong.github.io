## 서론

Apache에 MPM(Multi-Processing Module)이라는 개념이 있습니다. Apache 서버는 기본적으로 멀티 프로세스 방식이며 요청을 처리할 때 아래의 세 가지 유형으로 구분됩니다.



## MPM이란?

Apache가 받은 요청을 처리하기 위해 'child processes'에게 분배하는 방식입니다.

- Prefork MPM
- Worker MPM
- Event Driven



## Prefork MPM

**자식 프로세스가 싱글 쓰레드로 동작하며 요청당 하나의 프로세스가 처리**합니다.

- 프로세스간 메모리 공유가 없으므로 안정적
- Worker MPM에 비해 메모리 사용량 높음
- Idle Process 유지
- 다량의 동시접속시 자원사용량 매우 높음

![제목 없음](https://user-images.githubusercontent.com/41600558/97145201-299d0400-17a9-11eb-9d86-589800b938c0.png)



## Worker MPM

**자식 프로세스가 멀티 쓰레드로 동작하며 각 요청당 하나의 쓰레드가 처리하는 방식**입니다.

- Prefork MPM에 비해 메모리 사용량 적음
- 스레드간 메모리 공유 가능(스케쥴링 필요)
- Idle Thread 유지
- 동시 접속에 유리



## Event Driven

고정 프로세스만 생성하고 그 프로세스의 내부에서 비동기 방식으로 요청을 처리.

nginx에 의해 떨어지는 퍼포먼스로 인해 Apache 2.4버전부터 추가됨

- 속도가 가장 빠름
- 동시접속에 유리



## 결론

높은 확장가능성(Scalability)이 필요하면 **Worker MPM**

안정성과 호환성이 필요하면 **Prefork MPM**

두 방식의 속도는 비슷



## 참조

- [https://chobokkiri.tistory.com/65](https://chobokkiri.tistory.com/65)