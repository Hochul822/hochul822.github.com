#### 자바 스레드
스레드는 병렬 프로그래밍을 위해 나온 개념입니다. 프로세스 하나당 하나의 일만 처리하게 되면은 대기 시간이 많아집니다. 프로세스를 필요한만큼 잘라서 각각 다른 일을 시킬 수 있으면 좋겠죠? 한 프로세스 내에서 각기 다른 일을 하는 게 스레드입니다.

프로세스 - 하나의 실행된 프로그램
스레드 - 여러개로 나눠서 동시에 일하는 프로그램

스레드의 뜻은 '실'인데요. 실타래들이 엮어서 각자 일을 한다고 생각하면 이해가 되겠지요.


#### 스레드의 종류
자바의 스레드는 크게 메인(main)과 보조(sub) 스레드가 있습니다. 일반적으로 실행되는 프로그램은 메인 스레드에 있는 셈이지요.


자바에서 스레드를 쓰는 방법은 3가지가 있습니다.
사용하려는 곳에서 만들기, 클래스 상속 받아서 만들기, Runnable 구현해서 만들기인데
순서대로 알아보도록 하겠습니다.

#### 스레드 단순 실행

스레드에서 실행하고자 하는 명령은 run 안에 적어줘야해요.
스레드는 꼭 start()를 해줘야 만들어집니다.

```java
Thread thread = new Thread() {
    @Override
    public void run() {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread());
    }
};

thread.setDaemon(false); // 데몬 스레드일 경우 다른 일반 스레드가 종료하면 종료를 한다.
thread.start();
```

##### Thread 클래스를 상속 받아서 사용

스레드를 매번 선언하고 실행하는 건 가독성에 좋지 않겠죠?
보통은 스레드를 상속받거나 Runnable을 구현해서 사용합니다.
둘은 기능상 차이는 없습니다.

Thread는 클래스이고, Runnable은 인터페이스이기에, Thread를 상속받으면 다른 클래스를 상속 받을 수 없지만, Runnable은 인터페이스이기에 필요할때 덧붙일 수 있죠.

```java
class StartThread extends Thread {

    @Override
    public void run() {
        System.out.println("Start Thread : " + Thread.currentThread());
    }
}
```

```java
for (int i = 0; i < 10; i++) {
      StartThread s = new StartThread();
      s.start();
}
```


##### Runnable을 구현해서 실행
```java
class RunThread implements Runnable {

    @Override
    public void run() {
        System.out.println("Runnable Thread : " + Thread.currentThread());
    }
}
```

```java
Thread runnable = new Thread(new RunThread());
runnable.start();
```
