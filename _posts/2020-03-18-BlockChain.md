# 블록체인(Block Chain)

### 블록체인이란?

​	`블록`이라고 하는 소규모 데이터 블록들을 **P2P** 방식을 기반으로 연결. 생성된 체인 형태의 분산 데이터 저장환경에  관리 데이터들을 분산 저장하여 누구라도 임의로 수정할 수 없고 누구나 변경의 결과를 열람할 수 있는 **분산 컴퓨팅** 기술 기반의 원장 관리 기술.

​	변경되는 데이터를 모든 참여 노드에 기록한 변경 리스트로서 분산 노드의 운영자에 의한 임의 조작이 불가능하도록 고안.

​	

## 블록과 체인 구현

_________

- 블록 - 관리 대상 데이터
- 체인  - 연결 고리

구조체(객체) 값에 링크드 리스트로 엮는 모습과 비슷

다만, 실제로는 포인터 참조가 아닌 해시값 참조로 연결되어 있음.

해시의 이해가 필요.



### 해시함수

​	임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수.

- 특성
  - 입력값에 무관하게 고정된 길이의 알 수 없는 난수가 결과로 출력
  - 입력값이 한글자만 바뀌어도 완전히 다른 결과가 출력
  - 복호화 불가능
  - 같은 입력값에는 항상 같은 결과값

### 블록 해시

​	블록해시에서 해시는 해시함수를 의미.

### 블록 구현

​	블록은 구조체로 표현.

- 필수 변수
  1. 이전 블록의 해시값을 기억할 변수
  2. 현재 블록에서 간직해야 할 데이터

### 체인 구현

​	블록끼리의 관계는 해시값을 통해 구현.

​	이전 블록의 해시값을 현재 블록이 기억함.

- 제네시스 블록 = 최초의 블록

  이전 해시값에 `empty 값`( ≠ nul l) 혹은 random값을 넣어줌.

## 

## 작업 증명(Proof Of Work, POW)

__________

### 작업증명의 이유

​	블록 내 데이터의 위변조를 방지하기 위해 블록 갱신에 소요되는 비용을 향상 시켜야 함.

### Nonce

​	비트코인 창시자인 사토시 나카모토(Satoshi Nakamoto)가 쓴 비트코인 백서에 나오는 용어.

​	블록체인은 다수의 거래내역을 모아 하나의 블록을 구성하고, 그 블록을 대표하는 해시값을 생성하여 다른 블록과 체인처럼 연결.

​	블록을 대표하는 해시값인 블록해시를 생성하려면, 일정한 조건을 만족해야 함. → 작업증명 조건

​	 ( => 블록 난이도에 따라 자동으로 설정된 '목표값'보다 더 작은 블록해시값을 찾아야 한다는 제약조건)

​	해시는 랜덤하게 생성되기 때문에, 수 없이 많은 연산을 반복해서 미리 정해진 목표값 이하의 해시값이 나오도록 해야 함

​	이 때에 랜덤한 해시값을 생성할 수 있도록 매번 사용하는 임시값이 **Nonce**

​	

## 해시 활용 사례

___________

### 공개 키 암호 방식(PKC)

​	공개 키 암호 방식은 하나의 공개 키와 하나의 개인 키로 이뤄진 한쌍의 키를 사용하는 암호화 시스템

​	가장 기본적인 방법인 대칭형 암호화보다 안전.

​	공개 키를 통해 데이터를 암호화하고, 상응하는 개인 키를 통해 데이터 해독.

### 디지털 서명

​	암호화와 결합하여 고유한 디지털 지문으로 사용될 수 있는 해시값을 생성.

​	디지털 데이터의 진위성을 검증.

​	해싱, 서명, 검증 세 가지 기본 단계로 이뤄지는 경우가 많음.

​	데이터 무결성, 진위성, 부인 방지의 결과를 얻기 위해 사용.

`무결성` : 데이터가 전송간 변경되지 않음

`진위성` : 해당 데이터가 다른 이가 서명한 것이 아님을 확인할 수 있음

`부인 방지` : 서명자가 서명한 후에는 서명 사실을 앞으로 부인할 수 없음 

- 데이터 해싱

  메시지나 디지털 데이터를 해시화.

  해싱 알고리즘을 통해 데이터를 제출하여 해시값을 생성.

  항상 고려되는 것은 아님.

  개인 키를 사용해 해시되지 않은 메시지에 서명할 수도 있음.

- 서명

  정보가 해싱된 다음, 메시지 발신자의 서명이 필요. 공개 키 암호 방식.

  기본적으로 해시된 메시지는 개인 키로 서명되며, 메시지 수신자는 상응하는 공개 키(서명자가 제공한)를 통해 유효성을 확인.

- 검증

  수신자는 공개 키를 통해 디지털 서명의 유효성을 확인할 수 있음.

  수신자가 사용하는 공개 키는 발신자의 개인키에 상응하는 것임.

  개인 키는 안전하게 보관해야 함.

  

### 디지털 서명의 한계

- 알고리즘

  신뢰할 수 있는 해시함수와 암호화 시스템을 선택하여야 함

- 구현

  알고리즘이 좋지만, 구현지 좋지 못하다면 디지털 서명 시스템은 결점이 생김

- 개인 키

  개인 키가 유출되거나 손상되면, 진위성과 부인 방지 속성이 더는 유효하지 않음

### 전자식 서명과 디지털 서명 비교

​	디지털 서명은 문서와 메시지에 전자식으로 서명하는 방법을 의미하는 특정한 종류의 전자식 서명과 관련

​	→ 모든 디지털 서명은 전자식 서명

​	→ 전자식 서명이 언제나 디지털 서명인 것은 아님

​	주된 차이점은 인증 방법

​	디지털 서명은 해시함수, 공개 키 암호 방식, 암호화 기법 등의 암호 시스템을 사용.	