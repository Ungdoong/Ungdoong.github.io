## Thread Safe

**스레드 안전(Thread Safe)**은 멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻합니다.



## Thread-Safe를 지키기 위한 방법

1. **Re-entrancy**

   어떤 함수가 한 스레드에 의해 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하더라도 그 결과가 각각에게 올바로 주어짐

2. **Thread-Local Storage**

   공유 자원의 사용을 최대한 줄여 각각의 스레드에서만 접근 가능한 저장소들을 사용함으로써 동시 접근을 막음

3. **Mutual Exclusion**

   공유 자원을 꼭 사용해야 할 경우 해당 자원의 접근을 세마포어 등의 락으로 통제

4. **Atomic Operations**

   공유 자원에 접근할 때 원자 연산을 이용하거나 '원자적'으로 정의된 접근 방법을 사용함으로써 상호 배제를 구현할 수 있음



## MUTEX를 이용한 동기화

```c
pthread_mutex_lock(&mutx);

// ... Critical Section

pthread_mutex_unlock(&mutx);
```



## SEMAPHORES를 이용한 동기화

```C
// A Thread

state = sem_init(&bin_sem,0,0);

while(number != 0)	wait...

// 데이터 처리

sem_post(&bin_sem);

 

// B Thread

sem_wait(&bin_sem);

// 데이터 처리
```

