# Event Driven Architecture



## Event Driven

______

특정 행동이 자동으로/순서에 따라 발생하는 것이 아닌 어떤 **이벤트**에 대한 반응으로 동작하는 디자인 패턴

- 분산 처리 시스템에 Event-Driven을 엮어 Loosely Coupled를 지원하고 유연성, 탄력성, 확장성을 추구



## Event Driven Micro service(EDM)

___________

MSA가 적용된 시스템에서 이벤트 로그를 보관하고 이를 기반하여 동작하도록 함

- 이벤트가 발생(데이터의 생성, 변경, 삭제 등)하면 해당 도메인 객체를 기반으로 이벤트를 생성
- 생성된 이벤트는 저장공간에 보관. 비동기 메시지 큐로 해당 이벤트에 관심이 있는 서비스들에게 전달
- 이벤트를 확인한 서비스는 관련 로직을 수행
- 수행 중 오류가 발생 시 로그를 기반으로 retry 또는 rollback 수행

### Database Per Service

MSA의 Loosely Coupled, Polyglot Programming, 독립적 배포 주기 등을 달성하기 위한 핵심 키워드.

서비스별로 최적의 Database를 선택

- ### 적용시 고려사항

  - 중요 데이터 
  - 관계형 데이터베이스 장점 사용 불가
    - 테이블 조인을 통한 통합View
    - ACID 원칙에 따른 트랜잭션 기능
  - 시스템의 성격은 데이터의 CRUD 기능, 흐름, life cycle이 중요
    - DB 분리는 기존 데이터 흐름을 깨는 행위
  - 비용
  - 기존 DBMS에 최적화된 각종 세팅



### 효과

- 비즈니스 흐름에 따른 로직 수행
- 분산 트랜잭션 처리
- 서비스간 반정규화 데이터 동기 처리
- 적절한 시스템 내 통합
- 최종적 일관성