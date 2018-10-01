
### Hot, Cold Observable의 차이 및 예시
Observable의 개념에 익숙해질만 하면 접하게 되는 게 차가운(Cold), 뜨거운(Hot) 옵저버블이다. 옵저버블만 해도 복잡한데, 그 안에서 구분을 하다니 헷갈릴만하다. 사실 차이는 간단하다. 옵저버블은 데이터를 생산하는 것이라 했는데 필요할 때만 만드냐(cold), 필요없어도 계속 만드냐(hot)의 차이다.

Hot observable - 구독과 상관없이 계속 데이터 생산
Cold observable - 구독할 때만 데이터가 흘러들어옴

예를 들면, 폭포와 수도꼭지를 생각해보자.
폭포는 사람들이 보든 말든 계속 물이 떨어져 내린다.
반면 수독꼭지는 사람들이 꼭지를 열때만 물이 나온다.

폭포는 Hot Observable이고, 수도꼭지는 Cold Observable이라고 보면 된다.

###### *Cold Observable의 예*
interval은 구독할 때부터 동작한다. 밑의 예제에서는 두번째 구독은 0.5초 뒤에 시작하는데, 처음에는 First만 출력되다, 0.5초 뒤 Second도 출력되는 걸 알 수 있다.

```java
Observable<Long> cold = Observable.interval(200, TimeUnit.MILLISECONDS);

cold.subscribe(i -> System.out.println("First: " + i));
Thread.sleep(500);
cold.subscribe(i -> System.out.println("Second: " + i));
```

##### *Hot Observable의 예*
cold observable을 hot으로 바꿔줄 수 있다.
cold observable을 publish 하면 ConnectableObservable이 된다.
이를 connect하는 순간부터 계속 데이터를 생산한다.

```java
ConnectableObservable hot = Observable.interval(200, TimeUnit.MILLISECONDS).publish();
hot.connect();
hot.subscribe(i -> System.out.println("First : " + i));
Thread.sleep(500);
hot.subscribe(i -> System.out.println("Second : " + i));
```        

위의 코드를 보면 0.5초 뒤에 구독을 했지만 동일한 값이 출력되는 것을 알 수 있다.
구독 여부와 상관없이 데이터가 계속 생산되고 있었던 것이다.


#### Backpressure drop, backpressure buffer
- rxjava에서 흐름 제어를 할 때 필요하다.
- drop의 경우 일정량 이상의 데이터는 무시하고
- buffer는 subscribe할때까지 쌓아둔다.
- BackpressureStrategy는 5가지가 있는데 이에 따라 흐름을 처리하는 방식이 달라진다
  - LATEST : 가장 최근 데이터만
  - BUFFER : 다 쌓아둔다
  - DROP : 일정량만 쌓아두고 나머지는 버림
  - ERROR, MISSING


#### Observable 생성법
- Observable.fromCallable()
- Observable.just()
- Observable.defer()

#### 다양한 Observable


#### Flowable vs Observable
- Flowalbe - BackPressureDrop을 지원해준다.
- Observable -  Backpressure를 지원하지 않는다.
- cold observable vs hot observable
  - cold의 경우에는 Observable을 사용해도 overflowing으로부터 안전하다.
  - hot의 경우에는 안 쓰는 게 좋은데, observable의 consumer가 (많은 처리량을) 감당할 수 없을 수 있기 때문이다.


#### Maybe
- 값이 있으면 onSuccess 호출
- 값이 없으면 onComplete 호출

#### Single, Completable
- 값을 한개만 받을 때는 Single
  - Single은 OnSuccess 혹은 onError만 쓸 수 있는 타입이다.
  - OnComplte는 없다
- 값을 받지 않을 때는 Completable
  - Completable은 onComplete 혹은 onError만 쓸 수 있는 타입이다.
  - 의미없는 값을 return해야 될때 쓰면, onComplete가 강제되니 무의미한 return을 줄 일 수 있어 좋다
  - 예 : firebase에서 저장을 하고나서, 저장 확인용 onNext를 쓴 적 있는데 completable로 바꿔주면 좋지 않을까?


### Operator
map, zip, take, reduce, filter,
buffer, skip



### Subject
Observable과 Subject의 차이점은 내부에서 데이터를 주입하느냐, 외부에서 데이터를 주입받느냐의 차이다. Subject는 외부에서 주입받는다.

- PublishSubject
- ReplaySubject
- BehaviorSubject
- AsyncSubject



### Disposable


// TODO 예시 코드 추가


// 요약


// 참고 - https://mindorks.com/course/learn-rxjava/chapter/id/4/page/id/17

// http://developer88.tistory.com/86
// https://brunch.co.kr/@tilltue/18
