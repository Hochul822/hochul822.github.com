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
