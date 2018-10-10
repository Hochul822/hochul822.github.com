### Isolate -  Concurrency : Isolate와 Message 통신

Isolate는 메시지를 통해 서로 다른 Isolate끼리 통신을 한다.

원래의 Isolate와 새로 만들어진(스폰된) Isolate와 메시지를 주고 받으려면 ReceivePort와 SendPort가 필요하다.

각각의 역할을 알아보자.

- ReceivePort
  - 메시지를 받는 역할
  - SendPort를 갖고 있음.
  - ReceivePort 하나에 여러 SendPort가 있을 수 있다.
  - Isolate 하나당 하나씩은 있어야 된다.

- SendPort
  - 메시지를 보내는 역할
  - ReceivePort랑 항상 연결되어 있다.
  - SendPort를 쓰려면 ReceivePort를 먼저 만들어야한다.
  - 다른 Isolate에 넣을 수 있다.

DART - Isolate에서 메시지 주고 받기

```java
// Isolate가 생성된다.
echo(SendPort sendPort) async {
  var port = ReceivePort(); // 다른 Isolate와 통신하기 위해 ReceivePort를 만든다.
  sendPort.send(port.sendPort); // 어떤 포트에 연결되어 있는지 알려준다.

  await for (var msg in port) { // 이 Isolate에 메시지가 들어오면 처리를 한다.
    var data = msg[0];
    SendPort replyTo = msg[1];
    replyTo.send(data);
    if (data == "exit") {
      port.close();
    }
  }
}

// 메시지를 Isolate에 전달한다.
Future sendReceive(SendPort port, msg) {
  ReceivePort response = ReceivePort(); // SendPort를 쓰기 위해 ReceivePort를 만든다.
  port.send([msg, response.sendPort]);
  return response.first;
}
```

실행해보자.

```javascript
main() async {
  var receivePort = ReceivePort();
  await Isolate.spawn(echo, receivePort.sendPort); // Isolate 생성

  var sendPort = await receivePort.first; // 메시지를 보낼 포트를 정함
  var msg = await sendReceive(sendPort, "hello"); // 메시지 전달

  print(msg); // 메시지 출력
  msg = await sendReceive(sendPort, "morning");
  print(msg);

  msg = await sendReceive(sendPort, "exit");
  print(msg);
}
```

Isolate 내에 메시지를 보내고, 그 안에서 메시지 처리를 한 다음에 돌려주는 작업을 반복하고 있다.


### Stream, Future

Stream은 연속된 비동기적 이벤트를 말한다. 사용자가 언제 데이터를 줄지 정하는 게 아니라, Stream이 준비가 되면 데이터를 건내준다.

(Dart 공식 설명 - A stream is a sequence of asynchronous events. It is like an asynchronous Iterable where, instead of getting the next event when you ask for it, the stream tells you that there is an event when it is ready.)

아래 코드를 보자.

```javascript
Stream<int> countStream(int to) async* {
  for (int i = 0; i <= to; i++) {
    yield i;
  }
}
```

Stream은 yield를 통해 실행 전까지 계산을 지연(suspend) 한다.


Future는 지금 당장 끝나지 않는 계산을 말한다. 즉시 결과를 돌려주지는 않지만, 결국에는 (시간이 오래 걸리더라도) 결과를 돌려준다.

(Dart 공식 설명 - A Future represents a computation that doesn’t complete immediately. Where a normal function returns the result, an asynchronous function returns a Future, which will eventually contain the result. The future will tell you when the result is ready)

아래 코드를 보자.

```javascript
Future<int> sumStream(Stream<int> stream) async {
  var sum = 0;
  await for (var value in stream) {
    sum += value;
  }
  return sum;
}
```

스트림으로 들어온 데이터가 처리되기를 기다렸다가(await) 완료되면
await for를 통해 다 더한뒤, 결과를 처리해준다. 스트림에서의 일(복잡한 계산,네트워크,데이터베이스 조회 등)이 언제 끝날지 모른다. 다만 Future로 표시하면 언젠가 끝나면 그때 나머지 일을 하도록 한 것이다.


그럼 어떻게 Stream과 Future를 실제로 어떻게 쓸까?

```javascript
var stream = countStream(20);
var sum = await sumStream(stream); // await를 적어줘야 실제 값이 sum에 들어가게 된다.
print(sum);
```


#### Generator
sync*, async* 처럼 *을 뒤에 붙이면 제너레이터가 된다. 제너레이터를 쓰면 한번에 데이터가 나오는 게 아니라, 필요할때 필요한만큼만 데이터를 가져다 쓸 수 있다. 특히 비동기 제너레이터의 경우 자기 속도에 맞춰(own pace) 데이터를 넣거나 사용할 수 있다.


##### Isolate

Isoloate 어디다 쓸까?

다트는 기본적으로 싱글 스레드이지만, 백그라운드 작업을 하고 싶을 때는 compute를 통해서 한다.
백그라운드 작업으로 옮기는 걸 isolate라고 한다.

The compute function will run expensive functions in a background isolate and return the result.


##### Json

convert

https://flutter.io/cookbook/networking/fetch-data/

1. http package를 통해 데이터를 받아온다.
2. 받은 데이터를 json.decode를 통해 변환한다.
3. 변환한 데이터를 class로 해서 저장해준다.
4. FutureBuilder에서 데이터를 보여준다.
