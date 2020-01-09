# 190522(Wed)_SecurityTheory

### 주요용어

- 취약점 관리 : 해킹을 당했을 경우 해당 취약점을 보완하는 것

### 보안 위협 요소

- 자연적 위협
- 고의적 위협

### 계층적 방어 전략

- 관리적 방어 > 물리적 방어 > 논리적 방어
- 논리적 방어
  - Network Security : 네트워크를 통한 공격 방어
  - Host Security : 장치 내부 공격 방어
- 환경적 통제 : 환경 재해에 대한 방어

### 인증 시스템

- 인증 수단으로 사용 가능한 것
  - 알고 있는 것(Something you know)
  - 자신의 모습(Something you are) - 지문, 손 모양, 망막 등
  - 가지고 있는 것(Something you have) - 신분증, 스마트 카드, OTP 등
  - 위치 하는 곳(Somewhere you are)

- SSO ( Single Sign On) : 한 번의 로그인으로 하위 시스템 모두 이용 가능

### 방화벽 - 접근제어

- 로깅과 감사 추적, 필터링
- 한계 : 정상적인 서비스 포트에 대한 웜의 공격은 막을 수 없음

### 침입 탐지 시스템

- 기법
  - 이상 탐지
  - 오용 탐지
- 방화벽 보조 역할

### 침입 방지 시스템

- 침입 탐지 시스템과 방화벽의 조합

### 기타

- VPN(Virtual Private Network)
  - 임대 회선으로 암호화 하여 전송
- VLAN - 네트워크를 작은 단위로 나누어 사용

### 암호화 방식

- 전치법
- 치환법 - 시저 암호 등
  - 빈도 분석에 의해 파훼 -> 다중치환 도입
- 독일 에니그마
- Steganograpy - 일반적인 사물이나 컨텐츠에 정보를 은닉

### 바이너리 기반 암호화

- 컴퓨터의 도입으로 문자에 의한 암호화는 보안 수준 낮음 -> 바이너리 

- 스트림 암호
- 블록 암호
  - DES (Data Encryption Standard)
  - AES(Advanced Encryption Standard) 

## 시스템 보안

### 개요

- END-POINT 방어
- 6가지 핵심
  - 계정 관리 - 허락된 사용자
  - 세션 관리 - 허락된 시간
  - 접근 제어 
  - 권한 관리 - 특정 자원에 대해 권한 부여
  - 로그 관리
  - 취약점 관리

### 계정관리

- 그룹 계정 - 계정을 묶어 관리
- 패스워드 관리
- 패스워드 정책
- 패스워드 공격(크래킹)
  - 사전 대입 공격
  - 무작위 대입 공격
  - 레인보우 테이블을 이용한 공격 - 해시값으로 저장된 암호를 해킹
- 세션 관련 공격
  - 세션 하이재킹
    - 대응 - SSH와 같은 암호화 높은 것 사용

### 접근 제어

- 호스트 기반 정책 - 특정 호스트에서만 접근
- 계정 / IP 정책
- 시간 / 요일 정책
- 프로토콜 정책
- OTP 이중 인증

### 권한 관리

### 로그 관리

- 각종 이벤트에 대해 시간과 내용 기록
- 활용
  - 장애시 원인 분석
  - 공격 패턴 분석
  - 법적 책임의 증거
- AAA
  - Authentication
  - Authorization
  - Accounting
- 책임 추적성 (accountability)
  - 좋은 로그는 책임 추적성이 높음

### 취약점 관리

- 운영체제 업데이트
- 어플리케이션 업데이트
- 백도어
  - 트로이 목마 - 내부 악성코드
  - 탐지
    - 해시를 심어놓고 파일 변조 확인



## 윈도우 보안

### 윈도우 계정 정책

- 로컬 보안 정책 > 계정 정책 > 계정 잠금 정책 > 계정 잠금 임계값
- 로컬 보안 정책 > 보안 옵션 > 관리자 계정 이름 변경 or 게스트 사용 안함
- 로컬 보안 정책 > 보안 옵션 > 사용자 권한 할당 > 로컬 로그온 허용
- 로컬 보안 정책 > 보안 옵션 > 대화형 로그온 : 마지막 로그인 사용자~

### 로컬 보안 정책 (로그 관리)

- 로컬 보안 정책 > 로컬 정책 > 감사 정책

### 데몬 관리

- 데몬 : 백그라운드에서 항상 실행되고 있는 프로그램
- 데몬 실행 -> 포트 열림 -> 서비스 제공

### 방화벽 관리

- 인바운드 규칙
  - 패킷이 들어오는 것에 대한 규칙

### 레지스트리 관리

- 윈도우 실행에 필요한 모든 환경 설정 데이터를 모아놓은 저장소

### 윈도우 업데이트

- 윈도우 보안 점검 툴 - Baseline Security Analyzer

### 메모리 덤프 분석

- 메모리 덤프 분석 프로그램 - windbg



## 리눅스 관리

### 계정 관리

- 계정 리스트

  - cut -f1 -d: /etc/passwd

- UID 500 이상 계정 리스트

  - awk -F':' '{if($3>=500)print $1}' /etc/passwd

- 사용자 계정 개수 확인

  - cat /etc/passwd | wc -l

- UID 500 이상 계정 개수

  - awk -F':' '{if($3>=500) print $1}' /etc/passwd | wc -l

- 계정 추가

  - useradd, adduser 두가지 존재

  - sudo adduser newuser01

- 계정 삭제

  - userdel, deluser
  - sudo deluser newuser01

- 계정 전환

  - su newuser01

- 그룹 확인

  - cat /etc/group
  - cat -f1 -d: /etc/group

- 그룹 추가

  - groupadd, addgroup
  - groupadd -g 1001 group1
  - groupadd group2 : 알아서 gid 구현
  - groupadd -r group3 : -r 은 본래 하나씩 증가하던 gid를 반대로 줄여가며 매김

- 계정 추가 시 기본 파일 복사

  - /etc/skel 폴더에 파일을 넣어두면 계정 생성시 홈 디렉토리로 복사해줌

- 사용하지 않는 계정의 확인 및 제거

- 중복된 root 계정 존재 여부 확인

  - grep ':0:' /etc/passwd

### 리눅스 패스워드 크랙

- john the ripper
  - tar -zxvf john-1.9.0.tar.gz : 파일 압축 해제
  - Make
    - make clean linux-x86-64 : src폴더에서 빌드
  - 테스트용 유저 생성(3개)
    - sudo adduser ~
  - 패스워드 파일 복사
    - sudo cp /etc/shadow ./pass.txt
  - 패스워드 파일 권한 변경
    - sudo chown $whomai:$whomai ./pass.txt
  - 패스워드 크랙
    - ./john ./pass.txt
  - 사전 파일을 이용해 패스워드 크래킹
    - john --wordlist=dic.txt ./pass.txt

### 권한 관리

- drwxrwxrwx
  - d : 디렉터리, - : 일반 파일, l : 링크
  - rwx : 파일 및 디렉터리 소유자 권한
  - rwx : 파일 및 디렉터리 그룹 권한
  - rwx : 제 3의 사용자에 대한 권한

- UMASK 확인 및 변경
  - $umask 
  - $touch a.txt
  - $umask 027
  - $touch b.txt
  - $umask 022
- 파일 디렉토리 권한 변경
  - $mkdir test_dir
- 파일 및 디렉토리 권한 변경
  - $mkdir testdir
  - $chmod g+w testdir
  - $chmod o-x testdir
  - $chmod 777 testdir
  - $chmod 600 testdir

- 파일 및 디렉토리 소유권 변경
  - $chown taekjin testdir
  - $chown :taekjin testdir
  - $chown taekjin: testdir
  - $chown taekjin:test1 testdir

### 로그 시스템

- 파일 및 디렉토리 권한 확인
  - ls -al
- 로그 종류
  - UTMP - 기본
  - WTMP - 로그인 로그아웃 시스템 재부팅
  - Secure - 보안 관련
  - Syslog - 시스템 운영 전반
- Ubuntu 로그 저장 위치
  - /var/log/wtmp
  - /var/log/btmp
  - /var/log/auth.log
  - /var/log/syslog
  - /var/log/message
  - /var/run/log/utmp
- UTMP
  - 현재 로그인한 사용자의 상태 정보
  - utmp 데몬이 로그 기록
  - 현재 접속한 사용자
    - $w / $who / $users
- WTMP
  - 성공한 로그인/로그아웃, 시스템 부팅/종료 등에 대한 이력
  - $last
- BTMP
  - 로그인 실패 이력 / 해킹시도
  - $lastb
- Lastlog
  - 여러 계정에 대해 최근 접속 이력
  - $lastlog
- Secure (auth)
  - 보안 관련 로그
  - 사용자/그룹 생성 삭제 등
  - $sudo cat /var/log/auth.log
- DMESG 로그 파일
  - 부팅 로그
  - $dmesg
- $history - 내가 입력한 커맨드 확인

### 접근 제어

- $ifconfig
- 웅영체제 레벨의 접근체어 - iptables 이용

### IPTABLES

- 패킷 필터링을 하는 커널의 netfilter에 대한 정책 결정
- 필터 체인 - 입력/출력/포워딩
- ACCEPT, EJECT, DROP
- 설치
  - $dpkg -l | grep iptables
  - $sudo apt-get install iptables
- 재부팅 시에도 정책 적용
  - $apt-get install iptables-persistence

### TCP Wrapper

