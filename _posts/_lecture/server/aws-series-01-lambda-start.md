앞으로 3회동안 서버리스로 앱 개발을 해보려고 합니다.  

1. AWS Lambda와 AWS Gateway 연결해보기

2. AWS Lambda와 DynamoDB 연결하기, Serverless 프레임워크 사용

3. AWS Lambda와 Flutter 연결하기까지 진행하고 마치도록 하겠습니다.  



사용할 서비스는 AWS Lambda, AWS API Gateway, Flutter 입니다.  



기존에 저는 파이어베이스를 1년 넘게 사용했었는데  복잡한 쿼리가 안 되는 치명적인 단점이 있더군요.  



파이어베이스가 처음 개발할 때는 정말 좋은데 클라이언트 단에 코드가 몰리는 경향이  



강하고 데이터베이스에 있는 데이터 관리가 쉽지 않아  다른 서비스를 고려하기 시작했습니다.  



그러다가 AWS Lambda를 알게 되었죠.  



서버가 있는지 없는지 신경 쓰지 않고 함수만 호출하면 되는게 람다입니다.



람다를 쓰게 되면 서버가 죽었을 때 살리는 걱정을 하지 않아도 되는 장점이 있었습니다.  



게다가 커스터마이징도 가능하구요.  



반면 좀 상대적으로 느리다고 합니다.  



바로 콜드 스타트와 핫스타트때문인데요.  



오랫동안 람다 함수를 쓰지 않으면 람다가 대기 상태에 있기에



반응속도가 느려진다고 합니다.  



어떤 언어를 쓰느냐에 따라 다르지만 0.2 ~ 0.5초 가량 걸린다고 합니다.  



언제 람다를 써도 되는지 한번 정리를 해보겠습니다.



##### AWS Lambda를 써도 좋은 곳 (람다의 장점)

- 비용 예측이 어려운 곳
- 조금은 느려도 좋은 앱
- 확장을 적은 노력으로 빠르고 하고 싶을 때



##### AWS Lambda 쓰지 말아야할곳 (람다의 단점)

- cpu 처리를 많이 하는 곳
- 실시간 멀티 플레이를 하는 게임 (모바일 턴제 게임은 가능함)
- 속도가 중요한 앱



제 경우엔 람다를 쓰는 게 적절하다고 생각했기에 람다를 쓰기 시작했습니다.





### AWS Lambda 시작하기

aws lambda는 콘솔에서 만들 수도 있고 cli 툴로 만들수도 있는데요.  

일단은 콘솔에서 만들어보도록 하겠습니다.

aws에 로그인하고 함수를 만드는 화면으로 이동합시다.



![alt text](https://user-images.githubusercontent.com/13621579/49022415-28878480-f1d8-11e8-901c-d05c397b64de.png)



create function을 누르면 람다 함수를 만드는 화면으로 넘어갑니다.



![alt text](https://user-images.githubusercontent.com/13621579/49022417-2b827500-f1d8-11e8-963e-4775cd59e685.png)



함수 이름(Name)은 ‘myHello'로 하고 프로그래밍 언어는 node.js를 쓰도록 하겠습니다.  



nodejs의 버전은 8.1로 하겠습니다. 8.1로 해도 예전 코드가 돌아가더군요.  



웹 상의 예제 코드가 낮은 버전이라고 해도 신경 쓰지 마시고 최신 버전 노드를  쓰면 됩니다.  



새로 roll을 만들거구요.



rollName은 ‘myLambdaRoll’로 하겠습니다.



람다 함수를 만들면 설정(lambda cofiguration) 화면으로 넘어오게 됩니다.



여기서 람다 함수를 변경해 줄 수도 있고, 람다와 연결할 트리거들을 정해줄 수 있습니다.



![alt text](https://user-images.githubusercontent.com/13621579/49022421-2e7d6580-f1d8-11e8-9d1b-862c30915e0f.png)



이 설정화면을 가장 많이 사용하게 될 건데요.

현재는 따로 연결된 함수는 없습니다.

오른쪽 상단을 보면 **Test**와 **Save** 버튼이 보이는데요.

람다 함수를 실행할 때 쓰입니다.

이제 화면을 내려서

실제 람다 코드를 보도록 합니다.

```javascript
exports.handler = async (event) => {
    // TODO implement
    const response = {
        statusCode: 200,
        body: JSON.stringify('Hello Lambda'),
    };
    return response;
};
```

어떻게 동작하는지 알고 싶다면 **Test** 버튼을 눌러보세요.

이벤트를 설정하는 화면이 나올거에요.



![alt text](https://user-images.githubusercontent.com/13621579/49022769-02161900-f1d9-11e8-9ef1-19619dce1e6f.png)



이벤트를 만들었으면 Save를 하고 다시 테스트를 해보세요.

응답이 아래처럼 오면 성공이에요.

```
Response:
{
  "statusCode": 200,
  "body": "\"Hello Lambda"\"
}
```



아래는 전체 실제 람다 에디터에서 실행해본 화면이에요.

소스 코드 밑에 response가 표시되어서 바로바로 결과를 확인할 수 있습니다.

index.js를 수정해보면서 람다가 어떻게 동작하는지 확인해보도록 합시다.

![alt text](https://user-images.githubusercontent.com/13621579/49022772-03dfdc80-f1d9-11e8-9ad0-b2e84d3d3e1e.png)



지금까지 가장 기초적인 람다를 만들었습니다.

람다를 바로 쓸 수는 없고, 외부에서 접근하려면 AWS API Gateway와 연결을 해주어야합니다.



### AWS API Gateway

aws api gateway는 api를 만들때 쓰입니다. aws 내부의 서비스와 외부의 연결 통로를 만들 때 쓰이죠. aws lambda도 외부에서 쓰려면 api gateway와 연결해야합니다.

##### 1. 새 api 만들기

![alt text](https://user-images.githubusercontent.com/13621579/49023914-997c6b80-f1db-11e8-8e53-79bf92d0bfd0.png)

api gateway를 쓰려면 새 api를 정해줘야합니다.

이름은 편한대로 정해주세요.



##### 2. 리소스 만들기

![alt text](https://user-images.githubusercontent.com/13621579/49023850-75208f00-f1db-11e8-81fb-9903e0936fcf.png)



리소스는 url을 말합니다.

리소스를 통해 api에 접근하게 되는거죠

여기서는 **create Resource**를 택해줄께요.



##### 3. 리소스 만들기 - 리소스 이름 정하기

![alt text](https://user-images.githubusercontent.com/13621579/49023861-794cac80-f1db-11e8-811f-ab9a06317688.png)



리소스 이름(resource name)은 user로 하겠습니다. 이제 url에 user가 추가되었네요.



##### 4. post 메소드 만들기

![alt text](https://user-images.githubusercontent.com/13621579/49023873-7ea9f700-f1db-11e8-9abd-af2ba324f6d2.png)



리소스를 만들었으면(url을 정했으면)

이제 메소드를 만들 겁니다. 메소드는 get, post, put, delete 등이 있는데 모두 rest 방식에서 쓰이는 것들입니다.

user를 클릭하고 **create method**를 눌러주세요.

 action을 고를 수 있는데 그 중에서 POST를 선택해줍니다.

그리곤 어떤 람다함수와 연결할지를 정해주면 됩니다.

![alt text](https://user-images.githubusercontent.com/13621579/49024446-d301a680-f1dc-11e8-99da-dd835d9d4173.png)





람다와 API 게이트웨이의 연결이 되었습니다.

연결을 했다고 끝이 아니에요. Deploy를 해야 외부에서 람다를 사용할 수 있습니다.

저도 디플로이 과정을 깜빡해서 엄청 삽질을 했었죠.

##### 5. API Deploy하기

![alt text](https://user-images.githubusercontent.com/13621579/49023894-8a95b900-f1db-11e8-8456-ee7f363054ca.png)







Actions탭에서 **Deploy API**를 골라주세요.



![alt text](https://user-images.githubusercontent.com/13621579/49023902-8d90a980-f1db-11e8-941f-b0d292c7fb89.png)





Deploy 설정에서는 이름만 정해주도록 할께요.



디플로이가 끝나면 **Stage**로 가봅시다.



Invoke URL이 생겼죠?



이 URL에다가  HTTP 메소드(Post)를 써주면 이제 완성입니다!



![alt text](https://user-images.githubusercontent.com/13621579/49024961-f5e08a80-f1dd-11e8-9cd2-83e036eb03db.png)







##### 7.  API 테스트 하기

저는 포스트맨을 이용해서 테스트를 해보겠습니다.

Method를 포스트로 바꿔주고 **Invoke URL**을 통해 요청을 보내 볼께요.



![alt text](https://user-images.githubusercontent.com/13621579/49024966-f9741180-f1dd-11e8-8886-3765a38bfe24.png)



생각했던대로 응답이 오네요.

실행 시간은 350ms 좀 걸리긴 하네요.



### 람다 알아두면 좋은 특징

- 콜드 스타트 : 람다는 요청을 한다고 해서 바로 실행되지 않습니다. 한번 실행 후 일정 시간이 지나면 대기 상태에 들어갑니다. 이 대기 시간은 사용 언어에 따라 다릅니다. python, node가 효율이 좋고 go도 상위권이었네요. 대기 시간 문제를 해결하려면 주기적으로 스케쥴링을 걸어주면 된다 합니다. 

- 상태 없는 함수 (stateless function) : 람다는 각각의 실행이 서로 분리 되어 있습니다. 이전의 람다 실행이 다음의 람다 실행에 영향을 미치지 않습니다. 그럼 DB연결은 매번 해야되는 걸까요? 아닙니다. 람다는 컨테이너 단위로 관리되고 컨테이너가 종료 되기 까지는 딱 필요한 부분(여기서는 람다 함수 호출)만 실행됩니다. 


```javascript
var dbConnection = createNewDbConnection(); // 컨테이너가 살아 있음 매번 실행되지는 않음. 

exports.myHandler = function(event, context, callback) { // 람다 호출할때마다 실행됨.
  var result = dbConnection.makeQuery();
  callback(null, result);
};
```

(출처 : [serverless-stack - what is lambda](https://serverless-stack.com/chapters/what-is-aws-lambda.html) )

### 마무리

AWS Lambda가 개념 자체는 간단한데

아무래도 콘솔 창에 익숙해지는 좀 걸리더군요.

앞으로는 콘솔 대신에 Serverless라는 프레임워크를 통해서 람다를 다뤄보도록 하겠습니다.

대부분의 개발자분들은 서버리스 프레임워크가 더 편할 거에요.
