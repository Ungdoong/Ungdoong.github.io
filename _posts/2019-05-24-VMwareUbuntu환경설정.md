# 190524(Fri)_VMware Ubuntu 환경설정

### server 설정

- sudo : 명령어 한줄에 대하여 수퍼유저 권한 부여
- su : 하나의 세션에 대하여 수퍼유저 권한 부여

```
$sudo su - root
$exit
```

##### 부팅시 자동으로 root계정으로 접속 명령어

- 시스템 설정 > 사용자 계정 에서 자동로그인 활성화

```
$gedit /etc/share/lightdm/lightdm.conf
autologin-user=root 로 변경
$gedit /root/.profile
아래 mesg n || true 주석처리
```

##### 소프트웨어 설치의 설정파일 변경

```
$cd /etc/apt
$mv sources.list sources.list.bak (기존 소프트웨어 설치리스트 백업)
$wget http://download.hanbit.co.kr/ubuntu/16.04/sources.list
 (새로운 sources.list 다운로드)
$apt-get update (설정 저장)
```

##### 네트워크 설정

- 서버의 아이피가 DHCP이므로 고정시켜야함

- 오른쪽위 아이콘> 연결편집> 유선연결1 편집 >IPv4 설정
  - 주소 : 192.168.111.100
  - 넷마스크 : 255.255.255.0
  - 게이트웨이 : 192.168.111.2
  - DNS 서버 : 192.168.111.2

##### 불필요한 네트워크 메시지 출력하지 않게 하기

```
$gedit /usr/lib/avahi/avahi-daemon-check-dns.sh
AVAHI_DAEMON_DETECT_LOCAL=0 으로 변경
```

##### 화면잠금 설정

- 시스템설정 > 밝기와잠금 에서 설정

##### 입력기 설정

- 오른쪽위 키보드 아이콘 > 입력기 설정 > 키보드 - 한국어 - 한국어(101/104) 삭제

##### vim 재설치

```
$apt-get -y install vim
```

##### 방화벽 활성화

```
$ufw enable
```

##### VMware Tools

```
$mount (마운트된 것 확인)
$cd /media/root/VMware\ Tools
$cp *.gz /tmp
$cd /tmp
$tar -xvf VMware~.gz
$cd vmware~/
$./vmware~.pl
```

##### 일시 정지 상태의 가상머신은 하드웨어를 추가하거나 변경할 수 없다.

⇒ 정상적응로 죵료된 상태의 가상머신만 하드웨어 변경이 가능