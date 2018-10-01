- Service
  - 일반 Service : 메인스레드에서 동작
    - UnBound Service
      - Sticky,
    - Bound Service



  - Intent Service : 새로운 스레드에서 동작


  ### Service

  서비스는 2가지 종류가 있다. 하나는 바인드된 서비스이고, 하나는 바인드 되지 않은 (unbind) 서비스이다. 바인드란 안드로이드의 컴포넌트와 서비스를 연결하는 것을 말하는데, 컴포넌트가 종료되면 서비스도 종료가 된다. 예컨데 액티비티와 서비스를 바인드할 경우, 액티비티가 종료되면 바인드된 서비스도 같이 종료된다.

  언바인드 된 서비스는 안드로이드 컴포넌트와 상관없이 동작하며, 정해진 작업이 완료된 뒤에 종료가 된다.


  #### Bound Service

  바인드된 서비스는 클라이언트-서버 개념을 생각하면 된다.

  클라이언트는 안드로이드 컴포넌트가 되고, 서버는 바인드된 서비스이다.

  클라이언트에서 서버에 요청을 하면, 서버가 요청을 처리해 클라이언트로 결과를 전달하듯이,
  액티비티에서 서비스에 요청을 하면, 서비스가 어떤 연산을 한뒤 액티비티로 결과를 전한다.

  서비스를 바인드 하려면 3가지 과정을 거쳐야한다.

  1. 서비스 클래스에 바인더 정의
  2. 서비스 클래스의 **onBind()** 에서 바인드해주기
  3. 액티비티에서 ServiceConnection을 만들고 connect 때와 disConnected일때 어떤 처리를 할지 정해준다.
  4. 액티비티에서 bindService(intent, connection, Context.BIND_AUTO_CREATE)을 통해, 액티비티와 서비스를 연결해준다.
  5. 연결된 서비스에 있는 함수를 실행해 결과를 가져온다.
  6. 액티비티 종료나 멈출시 연결을 종료해준다.


  #### UnBound Service




  ///


## Service
- Invoke by startService()
- Triggered from any Thread
- Runs on Main Thread
- May block main (UI) thread. Always use thread within service for long task
- Once task has done, it is our responsibility to stop service by calling stopSelf() or stopService()
## IntentService

- It performs long task usually no communication with main thread if communication is needed then it is done by Handler or BroadcastReceiver
- Invoke via Intent
- Triggered from Main Thread
- Runs on the separate thread
- Can't run the task in parallel and multiple intents are Queued on the same worker thread.


## Service vs IntentService
#### When to use?

The Service can be used in tasks with no UI, but shouldn't be too long. If you need to perform long tasks, you must use threads within Service.

The IntentService can be used in long tasks usually with no communication to Main Thread. If communication is required, can use Main Thread handler or broadcast intents. Another case of use is when callbacks are needed (Intent triggered tasks).

#### How to trigger?

The Service is triggered by calling method startService().

The IntentService is triggered using an Intent, it spawns a new worker thread and the method onHandleIntent() is called on this thread.

#### Triggered From

The Service and IntentService may be triggered from any thread, activity or other application component.

#### Runs On

The Service runs in background but it runs on the Main Thread of the application.

The IntentService runs on a separate worker thread.

#### Limitations / Drawbacks

The Service may block the Main Thread of the application.

The IntentService cannot run tasks in parallel. Hence all the consecutive intents will go into the message queue for the worker thread and will execute sequentially.

#### When to stop?

If you implement a Service, it is your responsibility to stop the service when its work is done, by calling stopSelf() or stopService(). (If you only want to provide binding, you don't need to implement this method).

The IntentService stops the service after all start requests have been handled, so you never have to call stopSelf().
