- Flutter Structure
  - Material App
  - Scaffold
  - Layout
    - Row, Column
    - GridView
    - ListView
  - UI
    - Container, Center
    - Flex, Expand
    - FlatButton
  - Route
    - Navigator
  - Value change
    - setState()
    - initState()
  - Stream, Stream Controller : 플러터에서 리액트 프로그래밍을 가능하게 한다.


### MaterialApp

### Scaffold

### Container, Center


### Row, Column
- crossAxisAlignment : Row나 Column 자체를 움직이고 싶을 때 씀
  -> **crossAxisAlignment: CrossAxisAlignment.start ** 이 Row 안에 있다면, Row는 가로로 배치되니, 세로 방향중에서 가장 위쪽에 놓이게 됨.
  -> **crossAxisAlignment: CrossAxisAlignment.end ** 이 Row 안에 있으면, Row는 가로로 그려지는 위젯이니, 세로 방향 중 가장 아래쪽에 놓이게 됨.


Row와 Column의 속성들을 안드로이드에 레이아웃 연관 지어서 설명한 글!
( https://proandroiddev.com/flutter-for-android-developers-how-to-design-linearlayout-in-flutter-5d819c0ddf1a )


### Flex, Expand

### ListView

### PageView

### GridView
그리드 형식(2 x 4, 3 x 2)으로 뷰를 만들고 싶을 때 사용한다.

생성방법은 GridView.count(정해진 수의 타일,fixed number of tile), GridView.builder(요청할때 만들어짐, created on demand), GridView.extend(각각의 타일이 최대 교차 방향 범위를 가짐, maximum cross-axis extent)등 다양하다.

기본 스크롤 방향은 수직(vertical)이다.

따라서 1행씩 배치가 되는데,
crossAxisCount로 1행에 몇칸이나 있을지 정하게 된다.



```java
GridView.count(
  crossAxisCount : 2,
  children: ListView.generate(

  );
)

```


#### Route, Navigator

가고자 하는 페이지는 Route로 지정을 한다!

```java
class MyApp extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Flutter Demo',
      home: PageSelectStart(),
      routes: <String, WidgetBuilder> {
       '/page': (BuildContext context) => MyPage("Page 1")
      },
      debugShowCheckedModeBanner: false,
    );
  }
}

```


#### 화면의 이동은?
route,
Navigator

#### 클릭 이벤트 받기
GestureDetector() 위젯
RaisedButton()의 onPressed 속성


#### 설계 어떻게 할까?

https://github.com/Norbert515/BookSearch/tree/master/lib
(플러터에 기본적인 데이터 베이스를 붙이는 예제이다)

위 링크에서는 설계를
data, model, pages, redux, utils, widget으로 나누었는데
data는 데이터베이스랑 리파지토리용
model은 데이터 담는 클래스
pages는 UI이고
widgets은 위젯 (사실 pages랑 widgets차이를 몰겠음)

redux는 상태관리
utils은 공통되는 것들을 다룸


#### Image 넣기
BoxFit속성을 넣어줘야한다.

Image.asset(gif.url, width: 600.0, fit: BoxFit.cover)

BoxFit.cover - As small as possible while still covering the entire target box.
BoxFit.contain - As large as possible while still containing the source entirely within the target box.


#### 스낵바 띄우기
```java
final snackBar = SnackBar(content:Text("Tap"));
Scaffold.of(context).showSnackBar(snackBar);
```

->  음? scaffold가 아님 동작 안 하는 느낌.

### BuildContext

Widget을 build할때 꼭 BuildContext를 포함하는데 ,**Widget build(BuildContext context)**  , BuildContext는 헷갈리는 개념이다. 오늘까지 이해한 바로는(2018.09.17)  BuildContext context는 현재 만드는 위젯의 context가 아니라, 부모의 context이다.

아래 코드에서 **Scaffold.of(context).showSnackBar()** 를 실행하면 에러가 난다.
왜냐면 참조하는 context가 Scaffold의 것이 아니라, 부모의 것이기 때문이다.

```java
@override
Widget build(BuildContext context) {
  // here, Scaffold.of(context) returns null
  return new Scaffold(
    appBar: new AppBar(title: new Text('Demo')),
    body: new Builder(
      builder: (BuildContext context) {
        return new FlatButton(
          child: new Text('BUTTON'),
          onPressed: () {
            // here, Scaffold.of(context) returns the locally created Scaffold
            Scaffold.of(context).showSnackBar(new SnackBar(
              content: new Text('Hello.')
            ));
          }
        );
      }
    )
  );
}
```

### App lifecycle
- initState()
- dispose()
- didChangeAppLifecycleState()


### Stream



### Platform channel - 안드로이드와 ios의 코드를 직접적으로 사용함.
- Fluuter가 클라이언트가 되고, 안드로이드와 IOS가 서버가 되는 구조


- 에셋 안드로이드, ios에서 접근하기 ( https://flutter.io/assets-and-images/)


### Internalization - 여러 언어 지원
꽤나 복잡하다.

Intl이란 모듈을 사용하는데, Intl은 메시지를 통해서만 데이터를 주고 받는다.

간단한 Message를 정의하고 사용해보자.

```java
String continueMessage = Intl.message("Hit any key to continue",
  name: "continueMessage",
  args: [context],
  desc: "Explains that we will not proceed furthur until "
      "the user presses a key");

print(continueMessage);
```

겨우 1줄 출력하는데 5줄 넘게 써야만 했다.
name,args,desc는 message를 만드려면 필수다 ㅠㅠ
또한 name와 Intl.message 변수의 이름이 동일해야된다.
```
String continueMessage
name: "continueMessage",
```

Intl.message에 변수를 넣을 수도 있다.

```java
greetingMessage(name) => Intl.message(
      "Hello $name!",
      name: "greetingMessage",
      args: [name],
      desc: "Greet the user as they first open the application",
      examples: const {'name': 'Emily'});

print(greetingMessage('James'));
```

intl의 message에
intl의 message들을 번역하려면 추출하는 작업을 해야된다.

```
// 추출하기
flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/l10n lib/demo_localization.dart
```

intl_messages.arb 파일이 만들어 졌는지 확인해보자.


- intl_messages.arb
```json
{
  "@@last_modified": "2018-10-02T13:15:36.083574",
  "title": "Hello world app",
  "@title": {
    "description": "The application title",
    "type": "text",
    "placeholders": {}
  },
  "hello": "Hello",
  "@hello": {
    "type": "text",
    "placeholders": {}
  }
}
```

intl_messages.arb를 복사해서
언어에 맞게 파일을 만들어주자. 언어에 맞게 번역하는 건 필수다!

- intl_en.arb
- intl_es.arb
- intl_ko.arb

이제 arb 파일을 바탕으로 다트 파일을 만들어보자.

```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n --no-use-deferred-loading lib/my_app_localization.dart lib/l10n/intl_*.arb
```

** messages_en.dart, messages_ko.dart, messages_es.dart ** 같이 언어에 맞게 파일이 만들어진다.



결론 : 플러터에서 여러 언어를 지원하려면 Intl.message 작성 -> arb로 추출 -> arb를 언어별로 생성 -> arb를 번역 -> arb를 플러터에서 쓰이게 변환해야한다.

간단하진 않다.

기타 - intl로 번역하는 게 계속 안되길래

pubspec.yaml에서 아래 부분을 지워봄.

```yaml
transformers:
- intl_translation:
$include: main.dart,demo_localization.dart
```

이 부분을 지우니 잘 된다~!

참고 - https://proandroiddev.com/flutter-localization-step-by-step-30f95d06018d

#### 플러터 출시하기
안드로이드
flutter build apk --release


#### 플러터 UI 참고 사이트

기본 플러터 UI로 앱을 만들어서 파는 사이트(이쁜 UI가 많다)
https://github.com/Norbert515/BookSearch/tree/master/lib

다른 사람들이 만든 플러터 UI를 gif로 볼 수 있는 곳
https://startflutter.com/


---
플러터 엔진에서 스레딩
https://github.com/flutter/engine/wiki/Threading-in-the-Flutter-Engine

플러터는 기본적으로 4개의 Task Runner로 나눠져 있는데 스레드와 비슷한 역할을 한다.

- Platform Task Runner
- UI
- GPU
- IO


---
Flutter의 강점과 단점

강점
- 핫리로딩 : 정말 큰 장점이다. 익숙해지기만 하면 UI 빌딩 속도를 미친듯이 향상시킬 수 있다.
- 다양한 해상도 대응이 용이 : 명확하게 패딩이나 마진을 지정하기만 하면 어지간한 기기 해상도는 대응된. SafeArea같은 위젯을 사용하면 아이폰 X의 Notch도 간단히 처리



단점
- 학습속도
- 알아보기 쉽지 않은 코드
- 자유로운 UI 배치에 조금 어려움 : 코드로 다 작성하다보니 유연성이 좀 떨어지는 느낌이다. 익숙해지니 많은 부분을 표현할 수 있지만 드래그 드랍으로 UI 만드는 거에 비하면 제약이 있음. UI간의 위치를 상대적으로 지정해서 배치하는 방식에 비하면 제약이 있음. 아직 이해도가 낮아서 이렇게 느끼는 걸수도?
