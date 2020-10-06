# Ethereum Private Network Guide 2



이번 가이드에서는 전 가이드에서 구축한 이더리움 private 네트워크가 정상 작동하는지 확인해보도록 하겠습니다.



## Account 생성

전 가이드에서 도커 컨테이너 내부에서 ethereum/client-go <생량> console 명령어로 geth console에 진입하였습니다. geth console에서 **eth.accounts**  명령어를 실행하면 아직 생성된 account가 하나도 없는 것을 확인할 수 있습니다.

아래의 명령어를 통해 새로운 account를 생성할 수 있습니다.

```shell
personal.newAccount('<비밀번호>')
```

account를 아래와 같이 3개 생성하였습니다.

<img width="441" alt="제목 없음" src="https://user-images.githubusercontent.com/41600558/94982758-3ba7d000-0578-11eb-8a6a-bcf935d31ed4.png">



## Mining 시작하기

채굴은 아래의 명령어로 시작할 수 있습니다.

```shell
miner.start(1)
```

<img width="442" alt="제목 없음" src="https://user-images.githubusercontent.com/41600558/94982793-7c074e00-0578-11eb-8083-202a372a0512.png">

잠시 기다리다 보면, percentage가 100이 되고 블록을 생성하기 시작합니다.(Successfully sealed new block)