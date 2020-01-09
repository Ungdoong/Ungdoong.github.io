# 190807(Wed)_Java\_이론

### Exception

- 자식이 부모보다 큰 Exception을 던지는 것 X(같거나 더 좁아지는 건 O)
- 부모가 처리하지 않은 Exception을 던지는 것 X

### 스레드(Thread)

- start() - 필수 메소드
- stop() - 되도록이면 사용 X

- run() - Thread의 작업 내용을 가지고 있는 메소드
  
  - callback 메소드:특정 조건이 만족되면 자동실행되는 메소드
- Runnable(implements) / Thread(Extends)

- ##### Thread(Extends)

  ```java
  //Thread 클래스 상속받는 방법:
  class Tiger extends Thread{
  	//thread가 해야할 작업 내용을 가지고 있는 메소드
  	//callback 메소드:특정 조건(start()가 호출되는 경우)이 만족되면 
  	//				  자동실행되는 메소드. 간접 호출.
  	public void run() {
  		System.out.println(getName() + " is running...");
  	}
  }
  
  public class TigerTest {
  	public static void main(String[] args) {
  		Tiger t1 = new Tiger();
  		//t1.run();//main thread에 의해 실행됨
  		t1.start();//thread 시작을 알리는 메소드.thread가 실행
  		
  		Tiger t2 = new Tiger();
  		t2.start();
  		
  		Tiger t3 = new Tiger();
  		t3.start();
  	}
  }
  ```

  

- ##### Runnable(implements)

  ```java
  class Lion implements Runnable{
  
  	@Override
  	public void run() {
  		Thread t = Thread.currentThread();//현재 실행중인 thread를 알아냄
  		System.out.println(t.getName() + " is playing...");
  	}
  }
  
  public class LionTest {
  	public static void main(String[] args) {
  		Lion l1 = new Lion();//l1이 thread는 아님. Runnable 객체
  							 //(Runnable implements 한 객체들)
  		Thread t1 = new Thread(l1);
  		t1.start();
  	}
  }
  
  ```



### Synchronized

- 같은 시간에 하나의 스레드만 접근가능
- 순차 실행