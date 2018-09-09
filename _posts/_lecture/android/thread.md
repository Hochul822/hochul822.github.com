- 안드로이드 프로세스

- 안드로이드 스레드의 종류
  - 포그라운드
  - 백그라운드

- 스데드 간의 통신

- 안드로이드 서비스


### 안드로이드 프로세스

안드로이드 프로세스는 안드로이드에서 앱이 실행되는 최소 단위이다. 기본적으로 앱과 프로세스는 1대1 관계지만 필요하다면 앱을 여러 프로세스에서 동시에 실행할 수도 있고, 반대로 하나의 프로세스에서 여러 앱이 실행될 수도 있다.

각 프로세스는 고유한 ID를 가지고 있다.

가장 바깥에 리눅스 프로세스가 있고, 안쪽에 JVM, 앱이 있다.

**리눅스 프로세스 > JVM > 앱** 순이다.

![alt text](img/thread/android-threading-process-architecture.png)


###### 안드로이드 프로세스 조회
adb shell에서 실행중인 프로세스를 조회해 볼 수 있다.

```shell
adb shell ps
```
무수히 많은 프로세스가 출력된다.
어떤 형식으로 적혀 있는지 보도록 하자.

|USER|PID|PPID|VSZ|RSS|WCHAN|ADDR|S|NAME|
|---|---|---|---|---|
|u0_a7|23162|1564|1388972|54140|SyS_epoll_wait|0|S|com.android.defcontainer|

UID : 안드로이드 프로그램별 ID, 프로그램이 설치되면 할당되는 ID 이다.

PID : 부모 프로세스의 ID, 안드로이드는 언제나 Zygote

PPID : 프로세스 ID

VSZ : 프로세스의 가상 메모리 크기 (virtual memory size of process in KiB(1024 bytes unit))

RSS : 실제 물리적으로 할당된 메모리양 (resident set size, the non-swapped physical memory that a task has used.)

PID는 여러 프로세스가 동일한 값을 사용하고 있다.

###### 앱 시작과 Zygote 프로세스
안드로이드의 리눅스 프로세스 처음 만들어질때 다음과 같은 과정을 따른다.

1. 리눅스 프로세스를 시작한다.
2. 런타임을 만든다.
3. Application의 인스턴스를 만든다.
4. 응용 프로그램의 시작점을 만든다.

매번 프로세스를 새로 시작하고, 런타임을 설정하는 건 오래 걸린다. Zygote는 이 작업을 줄이기 위해 만들어졌다. Zygote 프로세스는 특별한 종류의 프로세스이다. Zygote는 모든 앱에서 공유되는 핵심 라이브러리를 담았다.프로세스는 실행될 때 필수 라이브러리를 복사하지 않고, Zygote프로세스를 포크(Fork)한다.

###### 프로세스 우선 순위
우선 순위가 낮은 앱들은 메모리가 부족할 때 OS 상에서 먼저 종료된다.

우선 순위가 높은 앱부터 보자.

1. 포그라운드
  - 전면에 보이는 프로세스
2. 화면에 보이는 프로세스
  - 다 보이지는 않지만 화면에 드러나 있는 프로세스
3. 서비스
  - 백그라운드 실행 중이고, 화면의 구성요소와 연결 되어 있지 않은 프로세스
4. 백그라운드
  - 화면에 보이지 않는 액티비티, 대부분의 앱들이 가지는 레벨
5. 빈 프로세스
  - 활성화된 구성요소가 없는 프로세스. 다시 실행될 때는 대비해 유지하고 있지만, 시스템의 자원을 회수할때 가장 먼저 회수함.


###### 프로세스와 스레드의 차이
프로세스는 각각 주소 공간(address memory)가 있는 반면 스레드는 공유 메모리(a shared memory space)가 있다. 스레드는 공유 메모리를 통해서 데이터를 같이 쓰거나 읽을 수 있다.

![alt text](img/thread/processes-vs-threads.jpg)

안드로이드는 빠른 UI 반응을 위해 항상 스레드를 사용하고 있다. 스레드가 어떻게 쓰이는지를 알아야 깔끔하고, 만족도 높은 앱을 만들 수 있다.

### 안드로이드 스레드 통신
UI를 수정하는 건 메인 스레드(UI 스레드)에서만 가능하다.

여러곳에서 동시에 뷰를 수정하는 문제를 막기 위해서이다!

사용자가 앱을 켰는데, 화면이 갑자기 1초 간격으로 바뀐다거나, 앱을 켜는 순서에 따라 내용이 달라진다면 얼마나 당황스럽겠는가. 이를 막기 위해서 생겨난 원칙이다.

![alt text](img/thread/android-thread-textview.png)


스레드는 기본적으로 메시지를 통해 통신을 한다.

스레드 통신에는 크게 3개가 필요한데
핸들러, 루퍼, 메시지 큐이다.

각각의 역할을 짧게 설명해 보겠다.
결론부터 말하면 다 메시지를 전달하는 용도이다.

핸들러 : 루퍼와 스레드의 연결 통로
- 1. sendMessage() - 스레드에서 오는 메시지를 루퍼로 보낸다.
- 2. handleMessage() - 루퍼에서 온 메시지를 스레드로 전달한다.

루퍼 : 메시지 큐에 있는 메시지를 꺼내서, 핸들러에게 메시지를 처리하게 합니다.

메시지 큐 : 메시지를 쌓아놓았다가 언제 처리될지를 판단해서 루퍼로 보내서 처리하도록 한다.




![alt text](img/thread/android-thread-looper-handler.png)




##### 핸들러(Handler)
안드로이드 스레드는 그 안에서만 연산이 가능하지 다른 스레드와는 통신이 안됩니다. 하지만 스레드는 서로 데이터를 주고 받지 못하면 존재 의의가 없겠죠? 스레드 간의 통신을 위해 쓰이는 게 바로 '핸들러'입니다.

핸들러는 메시지를 보내고, 받는 역할을 합니다.

핸들러를 통해서 UI를 주기적으로 바꿔보도록 할께요.



```java
uiHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
        if (msg.what == 1) {
            timer.setText("" + seconds++);

            if (!stopTimer) {
                Message otherMsg = Message.obtain();
                otherMsg.what = 1;
                uiHandler.sendMessageDelayed(otherMsg, 1000);
            }
        }
    }
};
```

```java
Message msg = uiHandler.obtainMessage();
msg.what = 1;
uiHandler.sendMessage(msg);
```



##### Looper

##### Message queue



참고 - https://academy.realm.io/kr/posts/android-thread-looper-handler/
