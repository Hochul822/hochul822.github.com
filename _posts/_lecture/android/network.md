- http의 이해
  - url의 의미
  - 컴퓨터는 어떻게 통신하는가?
    - 브라우저에 주소를 입력했을 때 일어나는 일들

- retrofit의 이해
  - rest api란?
    - put, get, update, delete
  - 설정하기
  - interface 만들기
  - gson 추가하기
  - 데이터 요청해보기


##### URL (Uniform Resource Locator)이란?
택배를 보내려면 주소를 알아야하죠?

어디로 갈지를 나타내는 게 URL이에요.

**http://www.naver.com** 처럼 생겼습니다.

URL을 브라우저에 입력하면 사이트로 이동을 하는데, 브라우저가 URL을 보고 어디로 갈 지 알고 이동을 합니다.

URL은 웹사이트 구분할때뿐만 아니라, 메일이나 파일 주고 받기에도 활용이되요.


URL은 다음과 같은 구성으로 됩니다.

프로토콜 : http://
도메인 : www.korea.kr

URL 앞에 있는 프로토콜로 어떤 작업을 해야할지를 구분을 하죠.
http는 인터넷 통신을 나타내는 프로토콜이고 다른 프로토콜도 있습니다.

이메일 : mailto:somebody@mail.somehost.com
ftp :  ftp://id:pass@192.168.1.234/a.gif

###### IP 주소란?
URL은 http://www.naver.com 처럼 사람이 읽을 수 있게 되어 있죠.

하지만 컴퓨터는 영어나 기호를 읽을 수 없습니다.

숫자만 인식할 수 있죠.

컴퓨터는 영어로 된 주소를 **192.001.001.001** 처럼 숫자로 바꿔서 이해를 하죠. http://www.naver.com 은 알고보면 숫자나 마찬가지인거죠.

인터넷에는 무수히 많은 컴퓨터가 있고 IP 주소를 통해서 각각의 컴퓨터를 구분합니다.

그럼 브라우저는 영어로된 주소를 어떻게 IP 주소로 바꿀까요?

### DNS 서버
DNS 서버란 URL을 IP주소로 바꿀 수 있게 해주는 곳이에요.

브라우저에 URL을 치면, 그 URL이 제대로 된 건지, 어떤 IP랑 연결되어 있는지 확인을 해줘야합니다.

그 작업을 해주는 게 DNS서버에요.

주소를 치는 짧은 사이에 확인을 하는 거죠. 놀랍지 않나요?

이제 IP 주소를 확인했으면, IP 주소로 접속을 하면 되겠죠?

아쉽지만 아직 해야할 일이 많이 남았습니다!

![alt text](img/network/dns-and-how-it-works.png)






###### 레트로핏(Retrofit)이란?
레트로핏은 HTTP를 다루기 위한 라이브러리에요.

네트워크를 다루다보면 연결,연결해제,데이터 저장,데이터 파싱 등 다뤄야할 부분이 많아요.

하지만 레트로핏을 쓰면 이런 문제들은 신경 쓰지 않아도 되요.

레트로핏은 REST API를 통한 통신에 잘 맞는 라이브러리에요.

일단 REST API가 뭔지 알아보고, 레트로핏을 사용해보도록 할께요.

###### REST API란?
REST는 URL을 통해서 어떤 일을 하는지 알 수 있는 방식을 말합니다.

"유저 2의 정보를 주세요"를 컴퓨터가 서로 알 수 있는 방법으로 통신하려면 어떻게 해야할까요?

HTTP 메소드와 URL(주소)를 통해서 통신을 합니다.

HTTP 메소드는 인터넷에서 쓰이는 동작들을 정의해 놓은 것이에요. 컴퓨터를 쓰다보면 항상 필요한 작업이 있는데 읽기, 쓰기, 수정하기, 지우기입니다. HTTP 메소드는 인터넷 상에서의 읽기, 쓰기, 수정하기, 지우기를 담당해요.

HTTP 메소드의 종류에는 GET(읽기), PUT(수정하기), POST(쓰기), DELETE(지우기)가 있어요.

그럼 REST API는 어떤 방식을 만들어질까요?
HTTP 메소드 + URL 주소로 만들어집니다.

인터넷 서점의 API를 만들어볼께요.
책의 정보를 가져오는 API입니다.

[HTTP METHOD] [URL] 형식으로 되어 있어요.

**GET /book/love/1** 라고 하면 **/book/love/1** 에 있는 데이터를 가져오라는 뜻이 되지요.

- GET /book/love/1
  - 연애 코너의 1번째 책 정보를 가져오기

- POST /book/love/330
  - 연애 코너에 330번째 책 추가하기

- PUT /book/love/1
  - 연애 코너의 1번째 책 정보 수정하기

- DELETE /book/love/1
  - 연애 코너의 1번쨰 책 지우기

같은 URL이더라도 어떤 HTTP 메소드를 쓰느냐에 따라, 서버에서
다르게 반응을 하는게 REST API의 핵심이에요.

URL을 정할 때는 동사 대신에 명사를 쓰도록 합니다. 아무래도 HTTP 메소드 자체가 동사니 URL에 동사가 또 있는 건 중복이니 어색하겠죠.

REST API가 뭔지 알아보았으니 레트로핏을 사용해보도록 할께요.

### Retrofit

###### Retrofit 설정
우선 Gradle에다가 필요한 라이브러리를 추가해주도록 합시다.

레트로핏 라이브러리와 Gson-converter를 추가해주세요.

GSON은 자바 객체(object)를 JSON 형태로 바꿔주거나, 반대로 JSON 형태를 자바 객체로 바꿀 때 쓰여요. 우린 JSON 데이터를 쓸테니 GSON은 필수에요!

그럼 아래와 같이 라이브러리를 추가해주세요.

```gradle
dependencies {

  ....
  implementation 'com.squareup.retrofit2:retrofit:2.4.0'
  implementation 'com.squareup.retrofit2:converter-gson:2.4.0'
}
```

싱크가 성공적으로 되었나요?
다음 단계로 넘어가볼께요.

Retrofit을 통한 통신에는 4가지 작업이 필요해요. 순서대로 알아볼께요.
- Retrofit 객체 만들기
- API Interface 만들기
- 데이터 담을 클래스 만들기
- 비동기로 데이터 불러오기


###### 1. Retrofit - 객체 만들기

Retrofit을 쓰려면 다음과 같은 객체를 만들어줘야해요.
일단 따라해봐요.

```java
String url = "https://api.github.com";

Retrofit retrofit = new Retrofit.Builder()
                      .baseUrl(url)
                      .addConverterFactory(GsonConverterFactory.create())
                      .build();
```

**baseUrl(url)** 은 어떤 서버와 통신할지를 정합니다.
**addConverterFactory()** 은 받은 데이터를 어떤 Converter로 파싱할지를 정하는데, 여기서는 GSON을 써줘요. GsonConverterFactory.create()은 GSON을 쓴다는 의미입니다.

그리고 마지막에 **build()** 를 불러줘야 되요!

###### 2. Retrofit - API Interface 만들기

밑에 주소를 URL에 넣어보면 JSON으로 된 데이터가 주르륵 나오죠?
https://api.github.com/users/JakeWharton/repos

이런 식으로 된 REST API를 호출할 인터페이스를 만들어볼께요.

```java
public interface GithubService {
    @GET("/users/{user}/repos") // @GET, @PUT, @DELETE, @UPDATE 등 HTTP METHOD를 적어요.
    Call<List<Repo>> listRepos(@Path("user") String user);
}
```
첫줄부터 보면 class 대신 interface로 선언되어 있죠. Retrofit내부에서 인터페이스를 알아서 구현해서 함수를 만들어요. 그래서 꼭 interface 로 적어줘야해요!

두번째 줄을 보면 **@GET("/users/{user}/repos")** 라고 적혀있는데, HTTP METHOD를 특정 URL에 쓴다고 보면 되요.

아까 baseUrl을 **https://api.github.com/** 로 정했으니 **https://api.github.com/users/{user}/repos** 두번째 줄은  **/users/{user}/repos** 에서 GET METHOD를 호출해준다는 뜻이죠.

세번째 줄은 어떤 데이터 타입으로 HTTP 응답을 받아올지를 정하는 부분이에요. **Call** 은 retofit에서 데이터를 받을 때 쓰는 타입이구요, **```Call<List<Repo>>```** 은 **```List<Repo>```** 형태로 데이터를 받는다는 뜻이에요.

마지막으로 **@Path("user")** 는 url에 들어갈 값을 정하는 기호에요.

**@GET("/users/{user}/repos")** 을 보면 **{user}** 라고 적혀있었죠? **@Path** 에 있던 값이 입력됩니다.


###### 3. Retrofit - 데이터(JSON)이 담길 클래스 만들기
Repo 데이터를 담을 클래스를 만들어볼께요.

**https://api.github.com/users/{user}/repos** 를 브라우저에 쳐보면 엄청 많은 데이터가 넘어오는데요. 그 중 일부만 클래스화 시켰습니다.

```java
public class Repo {
    public int id;
    public String full_name;
    public String type;
    public String sort;
    public String description;
}
```

###### 4. Retrofit - 비동기로 데이터 불러오기
다시 액티비티로 돌아갈께요.

이제 데이터를 불러올 준비가 모두 끝났어요.

2가지만 더하면 서버에서 데이터를 무리없이 가져올 수 있습니다.

일단 아까 만든 인터페이스(interface)를 가지고 REST API를 쓸 수 있는 클래스(서비스)를 만들어요.

```java
GithubService service = retrofit.createService(GithubService.class);
```

위에서 만든 서비스를 보면 인터페이스에서 선언한 함수들을 쓸 수 있어요.


```java
service.listRepos("Hochul822");
```

아무일도 일어나지 않죠?

요청을 해야 데이터를 불러오는데, 아직 요청을 안해서 그래요.
데이터 요청은 **enqueue()** 라는 함수를 통해서 합니다.
**enqueue()** 는 비동기 방식으로 데이터를 받아옵니다.

```java
service.listRepos("Hochul822").enqueue(new Callback<List<Repo>>() {
    @Override
    public void onResponse(Call<List<Repo>> call, Response<List<Repo>> response) {

    }

    @Override
    public void onFailure(Call<List<Repo>> call, Throwable t) {

    }
});

```

이제 실행하면 데이터를 받아올 수 있어요.
받아온 데이터는 **```onResponse(Call<List<Repo>> call, Response<List<Repo>>)```** 콜백에서 쓸 수 있어요. response가 정상적으로 왔을때만 데이터를 받도록 해볼께요.

```java
service.listRepos("Hochul822").enqueue(new Callback<List<Repo>>(){

    @Override
    public void onResponse(Call<List<Repo>> call, Response<List<Repo>> response) {
        if (response.isSuccessful() ) { // response가 제대로 왔을 경우에
            List<Repo> list = response.body(); // body 값을 가져온다.
        }
    }

    @Override
    public void onFailure(Call<List<Repo>> call, Throwable t) {
        t.printStackTrace();
    }
});

```

특이한게 ```response.body()```에 있는 데이터(여기서는 json)을 특별히 수정 안 해줬는데도
List<Repo>로 바뀌어 있죠. 보통 HTTP BODY에 있는 값을 쓰려면 어떤 데이터 타입인지 체크도 해줘야하고, 자바 객체로 바꾸는 작업을 해줘야하는데, Retrofit을 쓰면 이런 일을 안해도 되서 편해요.

Retrofit으로 받아온 데이터를 ListView나 RecyclerView에 넣어주면은 되겠죠?



###### 기타 : Fake JSON API
일일이 JSON을 불러오는 서버를 만들지 않고도 JSON을 가져올 수 있는 서버이다.
주소 : https://jsonplaceholder.typicode.com/

url 창에 다음 링크를 눌러보자.

[https://jsonplaceholder.typicode.com/posts](https://jsonplaceholder.typicode.com/posts)

유저 데이터가 json 형식으로 날아올 것이다. 상당히 많은 양이다.
```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
  },
  {
    "userId": 1,
    "id": 3,
    "title": "ea molestias quasi exercitationem repellat qui ipsa sit aut",
    "body": "et iusto sed quo iure\nvoluptatem occaecati omnis eligendi aut ad\nvoluptatem doloribus vel accusantium quis pariatur\nmolestiae porro eius odio et labore et velit aut"
  },
 ......
]  
```

유저 하나의 데이터만 가져오고 싶다면 밑의 링크를 눌러보자.

https://jsonplaceholder.typicode.com/todos/1
```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

이처럼 실제 서버를 쓰지 않아도 json을 불러올 수 있다.


##### 기타 : 직렬화 - Serializable
직렬화는 자바 내부의 객체 또는 데이터를 자바 밖에서도 쓸 수 있게 바이트 코드로 바꿔주거나, 반대로 바이트 코드를 자바 객체로 바꿔주는 작업을 말합니다.




참고 : https://jongmin92.github.io/2018/01/29/Programming/android-retrofit2-okhttp3/
