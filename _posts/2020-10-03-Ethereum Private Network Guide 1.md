# Ethereum Private Network Guide 1



 VirtualBox과 Windows 10의 블루스크린 이슈에 의해 VirtualBox를 대신하여 Docker를 기반으로 이더리움 네트워크를 구축하기 위한 첫번째 가이드입니다.



## 도커 이미지 설치

```shell
$ docker pull ethereum/client-go
```



## genesis.json 생성

json 파일 생성 전, docker_ethereum(임의 설정 가능) 폴더를 생성합니다.

docker_ethreum 폴더 안에 genesis.json 파일을 생성한 후 아래의 코드를 입력합니다.

`genesis.json`

```json
{
  "config": {
        "chainId": 0,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip155Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x10",
  "extraData"  : "",
  "gasLimit"   : "9999999",
  "nonce"      : "0xdeadbeefdeadbeef",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

config 영역은 구성하고자 하는 이더리움 Network의 설정 부분이다. config를 제외한 key 값들은 16진수입니다.

- **chinId** : 현재 Chain을 식별하는 값. ([Replay Attack](https://en.wikipedia.org/wiki/Replay_attack)을 막기 위함)
- **homesteadBlock** : 블록체인의 Release버전을 나타냄
- **eip150Block, eip155Block** : Ethereum improved proposal(EIP) 각각 개선된 문제해결 방안을 추가할 경우 하드포크를 하게 되는데, 기본값은 0(하드포크 하지 않은 옵션값)

>  **Genesis.json 값 들이 전달되는 Go 세부 코드**
>
> https://github.com/ethereum/go-ethereum/blob/eaff89291ce998ba4bf9b9816ca8a15c8b85f440/params/config.go#L104

- **alloc** : Genesis 블록 생성 시 지정한 지갑에 할당된 양을 미리 채움

  > **alloc 사용 예제**
  >
  > ```json
  > "alloc" : {
  >     "2sa2a321h8353vba12as8965r3340huk3s6a2h6e" : {"balance" : "10"},
  >     ...
  > }
  > ```
  >
  > 

- **coinbase** : 블록 검증에서 획득한 보상이 전송되는 160비트 이더리움 주소. 제네시스 블록일 경우 이 값은 무엇이든 될 수 있음. 제네시스 다음 블록부터의 설정 값은 해당 블록의 유효성을 검사 한 채굴자가 설정한 주소가 됨.

- **difficulty** : 특정 블록의 난이도에 해당하는 스칼라 값. 제네시스 블록의 경우 이전 블록이 없으므로 난이도를 정의해야 하지만, 이후 블록부터는 이전 블록의 난이도와 타임 스탬프에 의해 계산 가능. 난이도를 통해 블록 생성 시간을 제어 가능.

- **extraData** : 해당 블록과 관련된 데이터를 포함하는 임의의 바이트 배열. 선택 사항이며 32 바이트보다 클 수 없음.

- **gasLimit** : 블록 당 소비할 가스를 제한하는 스칼라 값. 블록에 담을 수 있는 모든 트랜잭션의 gasLimit의 합계가 이 값을 넘지 않아야 함.

- **nonce** : 작업증명에 사용되는 64비트 문자열 해시. 이 문자열을 **mixHash**와 함께 사용. 검증된 토큰 값 n을 결정하기 위한 작업 증명 과정에서 일정 계산이 소비되었음을 증명하는 암호화 보안 임시 값.

  > **작업증명에 대한 수학적 조건을 명시한 논문**
  >
  > https://ethereum.github.io/yellowpaper/paper.pdf

- **mixHash** : nonce와 결합된 256비트 해시. 블록에서 충분한 양의 계산이 수행되었음을 증명.

- **parentHash** : 상위 블록 헤더의 Keccak 256 비트 해시. 실제 블록 체인을 형성하는 데 필요한 부모 블록에 대한 포인터로서 작용. Linked List의 메커니즘과 유사. 제네시스는 부모 블록이 없으므로 0

- **timestamp** : Unix의 time()과 동일한 스칼라 값. 블록 사이의 시간차를 난이도에 반영하기 위해 사용. 두 블록 사이의 시간차가 적으면 난이도가 높아짐.

> **참고사이트**
>
> - https://arvanaghi.com/blog/explaining-the-genesis-block-in-ethereum/
> - https://www.asynclabs.co/blog/blockchain/params-in-ethereum-genesis-block-explained/
> - https://medium.com/taipei-ethereum-meetup/beginners-guide-to-ethereum-3-explain-the-genesis-file-and-use-it-to-customize-your-blockchain-552eb6265145



## go-ethereum 설치

docker-ethreum 폴더 내에서 go-ethereum 저장소를 불러옵니다.

```shell
$ git clone https://github.com/ethereum/go-ethereum
```



## Dockerfile 작성

Dockerfile에는 ubuntu를 설치하고 호스트OS에 있는 genesis.json과 go-ethereum을 옮긴 후 geth를 make 합니다. 그리고 genesis.json을 이용하여 초기블록을 생성하는 작업을 합니다.

`Dockerfile`

```dockerfile
FROM ubuntu

COPY ./go-ethereum /home/go-ethereum
WORKDIR /home/go-ethereum/

RUN apt-get update
RUN DEBIAN_FRONTEND=noninsteractive apt-get install -y --no-install-recommends tzdata
RUN apt-get install -y build-essential libgmp3-dev golang git

RUN git checkout refs/tags/v1.9.22
RUN make geth
RUN cp build/bin/geth /usr/local/bin/

WORKDIR /home/eth_localdata
COPY ./genesis.json /home/eth_localdata

RUN geth --datadir "/home/eth_localdata" init /home/eth_localdata/genesis.json
```

Dockerfile의 옵션에 대한 내용은 따로 설명하지 않겠습니다.

해당 파일은 우분투에 geth에 필요한 것들을 옮긴 후 genesis 블록을 생성한 후 이미지 파일을 만들어줍니다.



## Dockerfile로 이미지 파일 생성

build 명령어를 이용하여 이미지 파일을 생성합니다. -t 옵션은 이미지 이름을 지정하는 것을 의미합니다.

```shell
$ docker build -t ethereum .
```

**마지막 마침표(.)는 필수입니다.**

올바르게 생성되었다면 `docker images`명령어를 통해 아래와 같은 화면을 볼 수 있습니다.

<img width="499" alt="제목 없음" src="https://user-images.githubusercontent.com/41600558/94983614-72351900-057f-11eb-8a66-6d69718a1804.png">



## docker-compoase.yml을 이용한 자동화

image를 컨테이너로 실행하기 위해서 도커의 run명령어를 사용하여도 되지만, 매번 실행할 때마다 옵션을 작성하는 건 비효율적이므로 docker-compose.yml을 이용하여 컨에니터 생성 과정을 자동화하겠습니다.

`docker-compose.yml`

```yml
version: '3'

services:
    eth0:
        image: 'ethereum'
        tty: true
        ports:
            - 8545:8545
            - 30303:30303
        environment:
            ENV: ETH0
            RPCPORT: 8545
            PORT: 30303
        container_name: eth0
        command: geth --networkid 921 --maxpeers 2 --datadir /home/eth_localdata --rpc --rpcport 8545 --rpcaddr "0.0.0.0" --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --port 30303
        working_dir: /home/eth_localdata
```

- **version** : 관련 링크 - https://docs.docker.com/compose/compose-file/
- **tty** : 해당 컨테이너에서 인터프리터를 제공할지에 대한 옵션
- **ports** : 포트포워딩
- **command** : 컨테이너가 생성되고 실행할 명령어

> **Geth 커맨드 옵션 상세** 
>
> https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options
>
> **0.0.0.0 ?**
>
> 로컬 시스템의 모든 IP 주소
>
> 소켓이 컴퓨터가 가진 사용 가능한 모든 IP 주소에서 수신 대기



## docker-compose.yml 실행

```shell
$ docker-compose up -d
```

-d 옵션을 통해 백그라운드 실행할 수 있습니다.

`docker ps` 명령어를 실행하면 아래와 같이 컨테이너가 실행되고 있음을 볼 수 있습니다.

```shell
# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                              NAMES
f0c5baa0d3e2        ethereum            "geth --networkid 46…"   15 seconds ago      Up 14 seconds       0.0.0.0:8545->8545/tcp, 0.0.0.0:30303->30303/tcp   eth0
```



## 여러 노드 추가하기

docker-compose.yml 파일의 services에서 매우 간편하게 노드를 추가할 수 있습니다.

`docker-compose.yml`

```yaml
version: '3'

services:
    eth0:
        image: 'ethereum'
        tty: true
        ports:
            - 8545:8545
            - 30303:30303
        environment:
            ENV: ETH0
            RPCPORT: 8545
            PORT: 30303
        container_name: eth0
        command: geth --networkid 921 --maxpeers 2 --datadir /home/eth_localdata --rpc --rpcport 8545 --rpcaddr "0.0.0.0" --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --port 30303
        working_dir: /home/eth_localdata
    eth1:
        image: 'ethereum'
        tty: true
        ports:
            - 8546:8546
            - 30304:30304
        environment:
            ENV: ETH1
            RPCPORT: 8546
            PORT: 30304
        container_name: eth1
        command: geth --networkid 921 --maxpeers 2 --datadir /home/eth_localdata --rpc --rpcport 8546 --rpcaddr "0.0.0.0" --rpccorsdomain "*" --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --port 30304
        working_dir: /home/eth_localdata
```

`결과`

```shell
# docker-compose up -d
Creating eth0 ... done
Creating eth1 ... done
```

```shell
# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                                              NAMES
f7338d0addab        ethereum            "geth --networkid 46…"   About a minute ago   Up About a minute   0.0.0.0:8545->8545/tcp, 0.0.0.0:30303->30303/tcp   eth0
fa7bfba07fa1        ethereum            "geth --networkid 46…"   About a minute ago   Up About a minute   0.0.0.0:8546->8546/tcp, 0.0.0.0:30304->30304/tcp   eth1
```



## geth 직접실행

컨테이너에 접속하여 직접 다뤄보도록 하겠습니다.

```shell
$ docker exec -it eth0 /bin/bash
```

위 명령어를 통해 node1의 컨테이너 내부 bash를 실행합니다.  다음, 아래 명령어를 통해 실행중인 geth에 접속합니다.

```shell
$ geth attach http://localhost:${RPCPORT} console
```

${RPCPORT}는 docker-compose.yml에서 설정한 환경변수입니다.

<img width="537" alt="제목 없음" src="https://user-images.githubusercontent.com/41600558/94984964-e6c28480-058c-11eb-9e54-9fe8541be551.png">

정상적으로 접속되면 위와 같이 명령어를 입력할 수 있는 콘솔이 실행됩니다.



## 노드간 연결

두 노드를 연결하기 위해 `admin.addPeer("<enode>")`를 사용합니다.

eth0의 geth 콘솔에 접속하여 `admin.nodeInfo.enode` 명령어로 enode를 확인합니다.

```shell
> admin.nodeInfo.enode
"enode://eed2e5fba6ed628dc2085861a59131eb23e7973e37e68ed044965c88f248e66c9f85317748a1075e4a98ce0c4aa2e901a89c92cc08b5dee9104fe3b84f6ceb10@13.125.72.116:30303"
```

여기서 `13.125.72.116`의 아이피는 컨테이너 아이피이므로 마지막 번호를 1로 변경하여 호스트를 가리키게 한다.

eth1의 geth 콘솔에서 `admin.addPeer("<enode>")`명령어를 이용해 연결합니다.

```shell
>admin.addPeer("enode://eed2e5fba6ed628dc2085861a59131eb23e7973e37e68ed044965c88f248e66c9f85317748a1075e4a98ce0c4aa2e901a89c92cc08b5dee9104fe3b84f6ceb10@13.125.72.1:30303")

true
```

`admin.peers` 명령어를 통해 정상적으로 peer가 추가되었는지 확인합니다.

```shell
> admin.peers
[{
    caps: ["eth/62", "eth/63", "eth/64", "eth/65"],
    enode: "enode://2c58213b9a42bdbe7556cafe5a63651c80a450ca568f912903faa3eefbb3482c2f7c0aaa8b213023d6bbe0cb98c00c4a2c6a3945e291875de7462abc3e3626e9@74.78.32.101:30303",
    id: "4f4fdeb44c438bd95b7678430009ef45ccd00e7bad16b1945b875752f8b2a727",
    name: "Nethermind/v1.8.105-0-bb0125f1a-20200930/X64-Windows/Core3.1.5",
    network: {
      inbound: false,
      localAddress: "172.18.0.2:46690",
      remoteAddress: "74.78.32.101:30303",
      static: false,
      trusted: false
    },
    protocols: {
      eth: "handshake"
    }
}]
```

eth1도 같은 방식으로 eth0에 연결해 준다.



여기까지 도커를 이용하여 이더리움 private 네트워크 설치를 완료하였습니다.

다음 가이드에서는 몇 가지 명령어를 통해 설치한 이더리움 private 네트워크가 정상 동작하는지 체크해보도록 하겠습니다.



## 참고사이트

- https://blog.naver.com/pjt3591oo/221242573887
- https://medium.com/returnvalues/docker%EB%A1%9C-%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0-5735b48ea43f