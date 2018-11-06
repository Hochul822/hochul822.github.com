### 목차

- Java Class, Interface, Static 배우기
  - Class
    - 틀만들기, 빵틀
    - 클래스 만들고, 변수 함수 만들어보기
    - 인스턴스의 개념
  - Interface
    - 클래스인데 비어 있는 클래스
  - Extends
    - Override
    - Overload
    - 다형성
  - Static
    - 미리 메모리에 불러서 사용하는 경우
  - Enum

### 들어가기

안드로이드를 배우려면 자바의 몇몇 개념을 필수적으로 알아야합니다.

그 중에 가장 중요한 게 클래스죠.

클래스는 처음에는 이해하기 난해할 수 있는데 익숙해질수록 잘 쓸 수 있게 됩니다.

그럼 클래스를 배워보도록 할까요?

### Class란?

클래스는 자바에서 함수, 변수 등을 묶어서 쓰는 '틀'이라고 생각하면 됩니다.

함수가 반복되는 일을 줄이기 위해 나왔듯이,

클래스도 반복을 최소화하기 위해서 나왔어요.

예를 들어볼께요. 빵을 만드는 경우를 생각해보죠.

빵을 하나를 만드는 건 쉽지만 수백개를 손으로 만드려고 하면은 어렵죠.

이때 우리는 빵을 찍어낼 빵틀이 필요해요.

클래스가 하는 일이 빵틀과 같아요.

여러번 해야할 일이 있는데, 일일이 함수를 만들어 쓰려면 효율적이지 않으니

클래스를 만들어서 반복을 줄이는 거죠.

![alt text](https://user-images.githubusercontent.com/13621579/47412889-d4881b00-d7a7-11e8-9c5d-291053a034a8.jpeg)

![alt text](https://user-images.githubusercontent.com/13621579/47412895-d7830b80-d7a7-11e8-9f9d-8955318fca71.jpeg)



```java
class Bread {
  int size = 100;
  String taste = "달다";
}
```

그럼 클래스는 어떻게 사용해야할까요?

아까 빵틀이 클래스라고 했죠?

빵을 만들어서 사용을 합니다.

아래와 같은 모양으로 클래스를 만들어요.

```java
public class Main {
  public static void main(String[] args) {

  // 클래스 이름 = new 클래스();
  Bread bread1 = new Bread();
  Bread bread2 = new Bread();

  }
}
```
조금 낯설 수 있는데 곧 적응될 거에요.

클래스를 만들 때는 항상 **new** 를 적어줘야해요.

실제 사용할때는 **이름** 을 가지고 사용하는데요.

여기서 이름은 **bread1** 이 되겠네요. 이름의 정확한 명칭은 **인스턴스** 인데 나중에 더 설명을 할께요.

```java
bread1.size;
bread1.taste;
System.out.println(bread1.size);
```

출력을 해보면 Bread에 있던 size의 값이 나오죠?

클래스는 안에 있는 변수를 가져오거나 바꿀 수 있어요.

첨이라 복잡하지만 익숙해지면 따로 관리하니 편하단걸 알 수 있을 거에요.

클래스에는 변수만 넣을 수 있는 게 아니에요.

함수도 넣을 수 있답니다.

```java
class Bread {
  int size = 100;
  String taste = "달다";

  void showTaste() {
    System.out.println(taste);
  }
}
```

클래스에 있는 함수도 변수와 비슷한 방법으로 사용할 수 있습니다.

```java
public class Main {
  public static void main(String[] args) {
      Bread br = new Bread();
      br.showTaste();

      br.taste = "고소하다";
      br.showTaste();

      String name = "Mike";
      String address = new String("Seoul");
  }
}
```

클래스에서 함수를 쓰는 걸 보면 어딘가 익숙한데요.

문자열, String이랑 비슷하게 생기지 않았나요?

사실 String도 클래스의 일종입니다. 사람들이 편하게 썼던 문자열도 기본 자료형이 아니었던거죠.

여태까지는 클래스를 만들고 변수에 값을 넣어줬는데

만들때부터 값을 넣어줄 수는 없을까요?

```java
class Bread {
  int mSize = 100;
  String mTaste = "달다";

  void showTaste() {
    System.out.println(taste);
  }

  Bread(int size, String taste) {
    this.mSize = size;
    this.mTaste = taste;
  }
}
```
가능합니다. 클래스에다가 Constructor(생성자)라는 걸 추가해주면 되는데요.

생성자라는 번역이 좀 어색해서 잘 안 와닿을거에요. 컨스트럭트(Construct)가 만든다는 뜻이잖아요.

컨스트럭터(Contructor)는 만들어주는 일을 하는 것이라고 보면 되요.

컨스트럭터는 클래스를 처음 만들때, 값을 넣고 싶은 경우에 사용합니다.

```java
public class Main {
  public static void main(String[] args) {
      Bread myBread = new Bread(500, "부드럽다");
      System.out.println(myBread.mSize);
  }
}
```

### Extend - 상속

클래스는 중복을 줄이기 위해 만들어졌어요.

중복을 줄이려면 만들어진 걸 재활용해야해요.

다시 쓰기 위해 나온 게 상속이란 개념인데요.

다른 클래스를 **확장** 해서 쓴다고 보면 되요.


```java
자식클래스 extends 부모클래스
```

기본 구조는 계속 쓰려는, 원형이 되는 클래스를 가져다가 쓰는 거라고 보면 되요.

```java
public class Person {
    String name;
    int weight;
    int height;
}
```

```java
public class Man extends Person{
    boolean isBold = false;
}
```

```java
public class Woman extends Person {

}
```

상속을 하게 되면 부모(상위의 클래스)에 있는 변수나 클래스를
모두 쓸 수 있어요.


### Interface - 인터페이스

인터페이스는 클래스랑 비슷하게 생겼는데 좀 다른 일을 해요.

인터페이스는 함수를 직접 구현하지는 않고, 다른 클래스에서 구현하도록 합니다.

인터페이스는 일종의 껍데기라고 할 수 있죠.

인터페이스의 역할은 크게 2개입니다.

1. 구현의 강제
2. 동작의 분리

먼저 **구현의 강제**를 설명해볼께요.

여행준비를 한다고 생각을 해봐요.

여행 가려고 하면 꼭 해야하는 일들이 있죠.

어떻게 이동할지 정하기(버스,기차,비행기), 어디 머물지 정하기, 입을 옷 챙기기 등 할 일이 많아요.

하지만 바빠다 보면 깜빡하기가 쉽습니다.

누가 안 까먹게 신경 써줬으면 좋겠죠?

그게 인터페이스가 하는 역할입니다.



다음으론 **동작의 분리**인데요.

모든 기능을 한 클래스에 넣으면 너무 클래스가 커져요.

복잡성을 줄이기 위해 인터페이스를 써서 필요한 기능들을 따로 분리해냅니다.

예시를 보면서 좀 더 알아보도록 하죠.

###### Interface 예시 - Bird 클래스

Bird 클래스를 만들어볼께요.

새의 종류는 오리,기러기,제비,타조 등 다양해요.  

어떤 새는 날수 있지만 닭이나 타조는 못 날죠.

따라서 Bird 클래스에 나는 동작을 넣는 건 맞지 않습니다.

동작에 따라 인터페이스를 만들고, 필요한 클래스만 인터페이스를 구현하도록 할께요.

새는 날거나, 달릴 수 있으니 FlyBehavior 인터페이스와 RunBehavior 인터페이스를 만들겠습니다.

```java
class Bird {
    void quack() {
        System.out.println("새가 꽥꽥");
    }
}

interface FlyBehavior { // 나는 행동을 따로 분리해 필요한 클래스에서 구현하게 합니다.
    void fly();
}

interface RunBehavior { // 달리는 행동을 분리해 필요한 클래스에서 구현하게 합니다.
    void run();
}
```

만든 인터페이스를 필요한 클래스에서 쓰도록 할께요.

오리는 날 수 있으니 Duck(오리) 클래스는 FlyBehavior를 구현하도록 할께요.

반면 타조는 날지 못하고 뛰어다니니 Ostrich(타조) 클래스는 RunBehavoir를 구현하도록 할께요.

```java
class Duck extends Bird implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("오리가 날아다녀요");
    }
}

class ostrich extends Bird implements RunBehavior {

    @Override
    public void run() {
        System.out.println("타조가 뛰어다녀요");
    }
}

public class Main {

    public static void main(String[] args) {
        Duck duck = new Duck();
        duck.fly(); // 오리 클래스에서는 fly()를 구현했습니다.
        Ostrich ostrich = new Ostrich();
        ostrich.run(); // 타조 클래스에선 run()을 구현했습니다.
    }
}
```

Duck과 Ostrich 모두 Bird 클래스를 상속했지만 동작은 달라지게 됩니다.

이렇게 필요한 기능을 인터페이스로 따로 빼내게 되면 클래스가 간결해지고 나중에 보기 편합니다.

하나 예를 더 볼께요. 이번에는 앱을 만드는 예시에요.



###### Interface 예시 - 앱 클래스

앱을 하나 쓰는 걸 생각해보죠. 모든 앱에 공통된 기능이 있겠죠? 앱을 열었다가 닫는 건 모든 앱에 필요합니다.

반면 터치나 인터넷을 쓰는 건 모든 앱에 필요한 건 아니죠.

터치와 네트워크는 따로 인터페이스로 빼놓았습니다.

```java
class App {
    void open();
    void close();
}

interface TouchEvent {
    void onClick();
    void onTouch();
    void onSwipe();
}

interface Newtwork {
    void onConnect();
    void onDisconnect();
}
```

메모앱은 글씨를 써야하니 터치 이벤트가 필요하겠죠. 그래서 터치 이벤트 인터페이스를 구현했습니다.

채팅앱은 인터넷이 있어야 다른 사람이랑 채팅을 할 수 있겠죠. 네트워크 인터페이스를 구현했습니다.

```java
class MemoApp extends App implements TouchEvent{

    @Override
    public void onClick() {
        System.out.println("메모 앱을 눌렀네요");
    }

    @Override
    public void onTouch() {
        System.out.println("메모 앱을 터치했네요");
    }

    @Override
    public void onSwipe() {
        System.out.println("메모 앱을 스와이프했네요.");
    }
}

class ChatApp extends App implements Newtwork {

    @Override
    public void onConnect() {
        System.out.println("네트워크에 연결되었어요. 채팅을 시작합니다");
    }

    @Override
    public void onDisconnect() {
        System.out.println("네트워크가 끊겼어요. 채팅을 종료합니다");
    }
}
```

똑같이 App Class를 상속받았지만 MemoApp이랑 ChatApp은 다르게 동작합니다.

```java
public class Main {

    public static void main(String[] args) {
        MemoApp memoApp = new MemoApp();
        memoApp.onClick();
        ChatApp chatApp = new ChatApp();
        chatApp.onConnect();
	}
}
```

이처럼 인터페이스를 쓰게 되면 

유연하게 프로그램을 만들 수 있습니다.

#### Enum

자바에서 Enum(이넘)은 여러 변수를 함께 묶어야할 때 쓰입니다.

근데 꼭 Enum은 필요한 걸까요? 그냥 변수를 쓰면 안 될까요?

요일을 나타내는 변수를 이때까지 배운걸로 만들어볼께요.

```java
public int MONDAY = 100;
public int TUESDAY = 200;
public int WEDNESDAY = 300;
public int THURSDAY = 400;
public int FRIDAY = 500;
public int SATURDAY = 600;
public int SUNDAY = 700;
```

변수를 7개나 적어야해서 보기 좋지 않고
한눈에 들어오지 않습니다.

반면 Enum을 보죠.

```java
enum DAY {
  MON,TUE,WED,THU,FRI,SAT,SUN
}
```

한 블록 안에 다 들어와 있어서 보기가 편하고 구분도 잘 됩니다.

Enum은 클래스랑 비슷하게 사용합니다.

```java
DAY today = DAY.MON;
```

### 마무리
안드로이드 개발에 필요한 자바의 80%는 배웠습니다.

자바의 다른 개념도 쓰이지만 안드로이드를 배우며 알아가는 게 좋습니다.

80대 20의 법칙 아시나요?

자바도 20년이나 된 언어다 보니 문법이 다양하지만

항상 주로 쓰이는 개념이 쓰입니다.

오늘 배운 개념만 숙지하면 안드로이드 개발을 시작하는데 무리가 없습니다!

그럼 다음에는 안드로이드의 UI와 레이아웃에 대해 알아보도록 할께요!
