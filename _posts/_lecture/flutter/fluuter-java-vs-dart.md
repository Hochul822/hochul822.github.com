### 자바에서 다트로 넘어가기

자바로 개발을 하다가 다트(Dart)로 작업을 하게되면 많은 공통점이 있단 걸 알게된다.

다트 공식 웹사이트에선 **familiar to many existing developers** 라고 말하는 데 과장된 이야기가 아니다.

대부분의 문법이 큰 차이가 없으며, 객체 지향 프로그래밍을 접해봤다면 빠르게 다트를 배울 수 있다.

다트와 자바엔 어떤 공통점과 차이가 있는지 알아보도록 하자.

#### 변수 선언
다트의 변수 선언은 자바랑 무척 유사하지만 차이가 있다. 숫자는 int,double만 써서 구분을 한다. 숫자를 구분하는데만 타입이 4개나 있는 자바와는 다르다. 또 동적 타입을 지원한다.

JAVA
```java
int num = 10;
float height = 175.3;
long area = 2500L;
String name = "Mike";
```

DART
```java
int num = 10;
double height = 175.3;
String name = "Mike";
var world = "Hello world"; // 동적 타입
var age = 78;
```

#### 배열 대신에 리스트

자바에선 배열과 리스트를 구분하지만 다트는 리스트만 사용하기에 좀 더 편하게 데이터를 다룰 수 있다.

JAVA
```java
int[] numbers = [10, 20, 30];
String[] countries = {"USA", "JAPAN", "KOREA"};
ArrayList<Integer> lists = new ArrayList<>();
lists.add(100);
lists.add(200);
lists.add(300);
```

DART
```java
var numbers = [10, 20, 30];
var countries = ["USA", "JAPAN", "KOREA", 10];
print(countries[0]);
```

### 함수
웹을 위해 만들어진 언어라 그런지, 자바스크립트랑 비슷한 측면이 많다. 타입을 명시해도 되고, 안 해도 된다.

JAVA
```java
int add(int a, int b) {
  return a + b;
}
```

DART - 타입 지정 안함.
```java
combine(a,b) {
  return a + b;
}

var result = combine(10, 20);
print(result);

var word = combine("hello", "world");
print(word);
```

DART - 타입 지정.

```java
int add(int a, int b) {
  return a + b;
}

int sum = add(15, 25);
print(sum);
```

Effective Dart에서는 타입을 지정하는 걸 권장하고 있다.
사실 플러터로 다트를 배우다 보니, 타입 지정을 안해도 되는지 몰랐었다..

### Class - 이름 있는 생성자(Named Constructor)
자바와 다트의 생성자를 같이 보도록 하자. 크게 다르지 않다.  

JAVA
```java
class Point {
  int x;
  int y;

  Point(int x, int y) {
    this.x = x;
    this.y = y;
  }
}
```

DART
```java
class Point {
  num x;
  num y;

  Point(num x, num y) {
    this.x = x;
    this.y = y;
  }

  Point.origin() {
    x = 0;
    y = 0;
  }
}
```

1가지를 제외하고는 큰 차이가 없다. **Point.origin()** 가 있냐 없냐의 차이다. **Point.origin()** 은 대체 어떤 역할을 하는 걸까?
**Point.origin()** 은 이름 있는 생성자(Named Constructor)로 x,y를 0으로 초기화 하는 역할을 한다.

```java
Point p = Point(10, 30);
print(p.x); // 10

var origin = Point.origin();
print(origin.x); // 0
```

이처럼 이름 있는 생성자(Name Constructor)는 여러 생성자를 만들거나 생성자 내에서 값 체크, 파싱 등 각종 작업을 할 때 쓰인다.

(다트 공식 사이트의 Named Constructor 정의 - Use a named constructor to implement multiple constructors for a class or to provide extra clarity)


DART
```java
class Point {
  num x;
  num y;

  Point(num x, num y) {
    this.x = x;
    this.y = y;
  }

  Point.origin() {
    x = 0;
    y = 0;
  }

  Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
    print('In Point.fromJson(): ($x, $y)');
  }
}
```

### Map
DART
```java
// Map에서 put의 경우 자바와 다르게, 이미 key가 없을때만 추가할 수 있다 .
Map<String, String> myMap = Map<String, String>();
myMap["myKey"] = "myValue";

print(myMap["myKey"]);
print(myMap["myKey2"]);

myMap.putIfAbsent("myKey", () => "my value");
print(myMap["myKey"]);
myMap.putIfAbsent("myKey2", () => "my value 2");
print(myMap["myKey2"]);
```

### Factory

### Isolate


### Stream


### Future


참고 - http://cogitas.net/from-java-to-dart/
