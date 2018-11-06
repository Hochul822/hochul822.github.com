### 목차

- 레이아웃(UI) 개념 및 종류
  - 뷰에 대한 소개
    - 정의 : 화면을 나타내는 최소 단위, 아이콘 따위
  - 레이아웃의 종류 및 구성 요소
    - xml이란?
    - Linear layout
      - width, height, orientation
      - id
      - weight
    - 상대 레이아웃(Relative layout)
      - width, height, rightOf, endOf
      - padding, margin
    - Grid layout

    - Constraint layout
      - UI에디터 사용법


- 각종 버튼과 앱을 연결해보기
  - 버튼의 종류
    - width, height, click, id
    - margin, padding
    - sp, dp 차이
  - 이벤트란?
    - 리스너, 함수 사용

- 안드로이드 라이프 사이클
----

### 들어가며

UI는 안드로이드의 가장 기본적인 부분이에요.

실제 사용자들이 앱을 쓸 때 매일 접하는 게 UI이죠.

UI는 레이아웃과 뷰로 되어 있습니다.

뷰는 버튼이나 텍스트등 앱 상의 아이콘을 가리켜요. 레이아웃은 전체 UI 구조를 잡을 때 쓰여요. 레이아웃은 뷰가 어느 위치에 넣일지 정합니다.

### Layout

레이아웃은 XML로 만들어져 있어요. XML은 처음에는 헷갈리는데 금방 익숙해질 거에요.
간단히 XML의 개념만 이야기하고 넘어갈께요.


##### xml(엑스엠엘) 설명
xml은 데이터를 저장하고, 보여주는 데 사용하는 문서입니다.
엑셀이나 워드를 보면 일정한 형식이 있는데
xml도 컴퓨터가 이해하기 쉽게 만든 문서에요.

xml은 웹의 기본인 html이랑 무척 비슷하게 생겼어요.


```xml
<coffeeshop>

  <americano>
    <ice>5</ice>  
    <coffee>150</coffee>
  </americano>

  <cafelatte>
    <ice>8</ice>
    <milk>50</milk>
    <coffee>130</coffee>
  </cafelatte>

</coffeeshop>  
```

```xml
xmlns는 namespace를 일컫는다.

namespace는 동일한 이름의 요소들이 충돌하는 걸 막기 위해 쓰인다.

똑같은 body여도 다르게 쓰일 수 있다.

첫번째 body는 html이지만
<body>
  <h3>html의 글자</h3>
  <p>html의 단락</p>
</body>

두번째 body는 신체 정보를 의미한다.
<body>  
  <weight>70</weight>
  <height>175</height>
</body>  

xml에서는 이런 충돌을 막기 위해 네임스페이스(xmlns)를 쓴다.

xmlns는 <요소이름 xmlns:prefix="URI"> 방식으로 쓰인다.
예컨데  <html:body xmlns:html="https://www.w3.org/TR/html5/"> 같은 식으로 쓰인다.
URI는 Uniform Resource Identifieres의 약자인데, 인터넷 상의 자원을 나타낼 때 쓰인다.
인터넷 주소도 URI의 하나이다.  

<root
  xmlns:html="https://www.w3.org/TR/html5/"
  xmlns:physical="http://codingsam.com/xml/physical/">

  <html:body>
    <html:h3>html의 글자</html:h3>
    <html:p>html의 단락</html:p>
  </html:body>

  <physical:body>  
    <physical:weight>70</physical:weight>
    <physical:height>175</physical:height>
  </physical:body>

</root>

```

안드로이드 xml도 동일한 구조를 가지고 있어요.
처음에 xmlns를 선언하고 그 밑에 요소(attribute)들을 놓는 구조에요.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.hello.MainActivity">

</RelativeLayout>
```


### 레이아웃 구조
![alt text](https://user-images.githubusercontent.com/13621579/47419429-21272280-d7b7-11e8-9549-b231a9e748d7.png)

안드로이드는 한 화면을 레이아웃이라는 걸로 표시해요.
레이아웃 안에 각종 뷰(버튼,텍스트,프로그레스바 등)이 있는 구조입니다.

레이아웃은 어떻게 UI를 만들지 정하는 뼈대라고 보면 되요.

레이아웃을 어떤 걸 정하느냐에 따라
안에 들은 뷰들은 큰 영향을 받아요.

안드로이드 레이아웃 종류는 정말 많은데요.
많이 쓰는 몇개만 알아볼께요.

#### Linear Layout
![alt text](https://user-images.githubusercontent.com/13621579/47419436-284e3080-d7b7-11e8-86b3-cddb67f3f20f.jpg)
리니어 레이아웃은 수직이나 수평으로 뷰를 배치하는 레이아웃입니다.

일단 한번 뷰들을 레이아웃 위에 놓아볼께요.

수직이나 수평으로만 계속 뷰가 놓이는 걸 알 수 있죠?

orientation을 통해 방향을 정해줍니다!

리니어 레이아웃의 가장 큰 특징은 weight 인데요.

리니어 레이아웃 안에 있는 뷰들의 비중을 정할 수 있어요.

3개의 버튼을 놓고 1:2:1로 weight를 맞춰볼까요?

화면 전체 해상도와 상관없이 버튼의 비중이 정해지게 됩니다.

다른 해상도를 지닌 기기로 바꿔도 그대로죠.

이때문에 리니어 레이아웃이 많이 쓰였습니다.

만들기 쉽고 편하거든요. 대신 속도 문제가 좀 있어요.

뷰의 위치를 정확하게 하는데도 좀 어려움이 있구요.


##### Linear Layout의 구성요소(attribute)
레이아웃은 항상 넓이(width)와 높이(height)를 정해줘야해요

```xml
android:layout_width="match_parent"
android:layout_height="match_parent"
```

layout_width안에 값을 바꿔주면 넓이가 변하게 됩니다.
layout_width에 넣을 수 있는 값은 3개가 있어요.

- match_parent
  : 바깥 부분에 맞게 꽉 채우기
- wrap_content
  : 내용물에 맞게 크기 조절하기
- xxdp (100dp, 150dp)
  : 해상도에 따른 값으로 조절하기


3개 중에 원하는 대로 골라서 쓰면은 되고,
가장 밑에 있는 레이아웃의 경우 보통 match_parent를 써요.  

```xml
android:layout_width="100dp"
android:layout_height="wrap_content"
```



#### Relative Layout
![alt text](https://user-images.githubusercontent.com/13621579/47419440-2dab7b00-d7b7-11e8-9225-5baddb8b6a1a.jpg)

다음으로 많이 쓰는 건 렐러티브 레이아웃인데요.

안에 있는 뷰들의 위치에 따라서

뷰의 위치가 바뀝니다.

마치 똑같은 사람이어도 누구한테는 '엄마' 누구한테는 '이모'이고 누구한테는 '고모'인것처럼

뷰도 위치에 따라 서로의 관계를 나타내게 됩니다.

**'버튼1의 오른쪽(rightOf)', '텍스트뷰1의 왼쪽', '레이아웃의 끝에(endOf)'** 등 종류가 무궁무진해요.

이건 해봐서 익숙해지는 게 답이랍니다 ㅠㅠ

렐러티브 레이아웃이 세밀하게 위치를 잡기에 더 좋고

속도도 리니어 레이아웃보다 나은 편입니다.

하지만 렐러티브 레이아웃도 이제는 물러나는 추세에요.

구글에서는 ** Constraint Layout ** 을 권장하고 있습니다.


#### TextView

뷰는 id를 통해 다룰 수 있는 게 가장 큰 특징이에요.

사실 텍스트뷰나 버튼이나 비슷한 특성을 공유해요.

또한 세밀한 위치 조정을 위해 padding과 margin을 자주 사용한답니다.

padding은 안쪽으로 들어오고,

margin은 바깥쪽으로부터 움직입니다.

한번 보여드리는 게 날 것 같네요.

![alt text](https://user-images.githubusercontent.com/13621579/47419450-31d79880-d7b7-11e8-90c9-a12d541ba73d.png)


#### Button
누를 수 있는 뷰에요. 상호작용을 위해서 사용한답니다.

#### EditText
문자 입력을 받을 수 있는 뷰에요.

로그인 화면 등을 만들 때 사용합니다.

#### ImageView
이미지를 표시하기 위해서 사용하는 뷰에요.

정말 많이 쓰입니다.

#### 뷰(View)와 액티비티(Activity)의 연결
레이아웃에 놓은 뷰를 어떻게 실제 코드와 연결을 할까요?

이 작업은 뷰에 있는 id를 통해 이뤄집니다.

```java
public class MainActivity extends AppCompatActivity {

    Button login;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        login = findViewById(R.id.login);
        login.setOnClickListener(event -> {

        });
    }
}
```

findViewById()가 보이나요?
이 함수를 통해 뷰의 ID를 찾은 뒤, 액티비티에서 사용할 수 있게 해줍니다.

버튼 객체를 생성하는 작업이라고 보면 되요.

```java
Button login = findViewById(R.id.login);
Button login2 = new Button();
```

실제 findViewId를 써서 찾은 뷰는
인스턴스처럼 쓸 수 있어요.

이제 뷰에 입력을 받는 법을 알아볼께요.

#### 이벤트 관리하기
뷰들은 사람이 뷰를 누를 때 반응을 해요.
사람이 뷰를 건드리는 걸 뷰 입장에서는 이벤트라고 하는데,
뷰는 이벤트가 생기기만을 간절히 기다리고 있습니다.

이벤트는 **'화면 터치'나 '마우스 클릭'** 같은 걸

뷰는 이벤트를 처리하기 위해서 **'리스너(listener)'** 를 씁니다.

동작 과정을 하나하나 설명해볼께요.

1. 뷰(버튼)과 리스너를 연결한다.
2. 뷰는 이벤트가 들어오기를 기다리고(listen) 있다.
3. 이벤트가 들어오면(화면 터치 등) 리스너는 정해진 일을 한다.

![alt text](https://user-images.githubusercontent.com/13621579/47419467-3bf99700-d7b7-11e8-95de-d2a52efe3935.jpeg)


#### 안드로이드 생명주기

![alt text](https://user-images.githubusercontent.com/13621579/47419480-4025b480-d7b7-11e8-8c9b-fbb6799267f1.png)

안드로이드는 라이프사이클이란 독특한 개념이 있는데요.

언제 앱이 시작되고 멈출지 모르기 때문에 생긴 개념이랍니다.

우리가 노트북이나 데스크탑을 쓸 때는 주로 안 자리에서 사용하고

방해받을 일이 상대적으로 적죠.

반면 스마트폰의 경우 중간에 전화도 오고, 쓰다가 내려놓는 일도 잦은 편이죠.

안드로이드 생명주기는 이런 상황을 다루기 위해 나온 개념입니다.

**앱을 완전히 끄는 경우, 잠깐 밑으로 내린 경우, 다시 앱을 쓰는 경우** 등에 여러 상황이 있죠.

크게 3가지만 기억하면 됩니다.

1. onCreate - 앱(액티비티)을 처음 시작할때 호출됨
2. onResume - 앱(액티비티)에 들어갈 때 호출됨
3. onStop - 앱(액티비티)을 백버튼, 홈버튼 등을 눌러 안보이게 했을때 호출됨, 백그라운드 상태일때 호출되는거죠.


```java
public class MainActivity extends AppCompatActivity {

    Button login;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Log.i("LifeCycle", "onCreate");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.i("LifeCycle", "onResume");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.i("LifeCycle", "onStop");
    }
}
```

### 마무리
레이아웃이랑 뷰는 처음에는 헷갈릴 수 있는 데

반복해서 쓰다보면은 점점 익숙해집니다!

### 혼자 해보기
코딩을 배우는 건 하루엔 안 되고 혼자서 충분한 연습을 해야해요.

오늘 배운 걸 바탕으로 간단한 프로그램을 만들어보세요.

중간에 막히더라도 극복하고 나면 보람을 느낄 수 있습니다.

그 과정에서 배운 게 오래 기억에 남죠.

아래 적은 프로그램을 스스로 만들어보세요!

- 로그인 화면 만들어 보기
- 계산기 만들어보기



참고 : Constraint Layout ( https://medium.com/@futureofdev/android-constraintlayout-%EC%89%BD%EA%B2%8C-%EC%95%8C%EC%95%84%EA%B0%80%EC%9E%90-62d2ded79c17 )
