# 190617(Mon)_NameServer

### PC의 네임 서버를 통한 IP 주소 획득 경로

1. www.nate.com 입력

2. PC가 리눅스라면, /etc/resolv.conf 파일에서 'nameser 네임서버IP'에서 로컬 네임서버 컴퓨터를 확인

3. 로컬 네임서버에서 www.nate.com의 IP 주소를 물어봄

4. 로컬 네임서버는 자신의 캐시 DB에 www.nate.com가 있는지 확인

5. 없을 경우, 로컬 네임서버는 'ROOT 네임서버'에 질의

6. 'ROOT 네임서버'도 모르는 경우 'com 네임 서버'의 주소를 알려줌

7. 로컬 네임서버는 'com 네임 서버'에게 질의

8. 'com 네임 서버'도 모를 경우, 'nate.com'을 관리하는 네임서버의 주소를 알려줌

9. 로컬 네임서버가 'nate.com 네임 서버'에 질의

10. 'nate.com 네임 서버'는 OOO.nate.com이라는 이름을 가진 컴퓨터의 목록은 모두 소유. www.nate.com의 IP 주소도 알기 때문에 IP주소를 알려줌

    ※ IP주소를 알려주긴하나 해당 IP의 컴퓨터가 실제 작동중인지는 모름

11. 로컬 네임서버는 www.nate.com의 IP 주소를 요구한 PC에 알려줌

12. PC는 획득한 IP주소로 접속시도

### 캐싱 전용 네임서버 구축

##### Server

```
#apt-get -y install bind9 bind9utils
```

- /etc/bind/named.conf.options 파일에 아래 내용 추가

  ```
  recursion yes;
  allow-query { any; };
  ```

  - VMware가 사용하는 가상머신의 네트워크 주소에 있는 모든 컴퓨터가 네임 서버를 사용할수 있게 설정하는 것

```
#systemctl restart bind9
#systemctl enable bind9
#systemctl status bind9
#ufw allow 53
#dig @<네임서버IP> <조회할URL>
or
#nslookup
>server 192.168.111.100
>www.nate.com
```

##### Client

```
#nslookup
```

