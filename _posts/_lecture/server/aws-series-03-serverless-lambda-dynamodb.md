### Serverless framwork

람다와 다이나모DB를 연결하기 전에 서버리스 프레임워크를 먼저 알아보도록 할께요.

웹 콘솔에서 aws 람다, api 게이트웨이 설정을 하는 건 불편하죠.

이를 코드로 편하게 배포할 수 있게 하는게 Serverless framwork [(링크)](https://serverless.com/) 입니다.

###### Serverless 설치

```shell
npm install serverless -g
```

설치가 잘 되었는지 확인하려면 **serverless -v **를 쳐보세요. 버전 확인이 되면 성공입니다. 

서버리스 프레임워크로 aws를 관리하려면 접근 권한을 줘야해요.



###### AWS IAM에서 자격증명(Credential) 만들기

1.우선 AWS에 접속해서 IAM 페이지에 들어가 주세요.

유저를 하나 추가합니다.

![alt text](https://user-images.githubusercontent.com/13621579/49515359-0aa5d800-f8da-11e8-90eb-25a4bab6aafe.png)

2. 유저를 추가할 때 꼭 **Programmatic access**를 체크해줘야 됩니다. (아래 그림 참고)



![alt text](https://user-images.githubusercontent.com/13621579/49515366-0ed1f580-f8da-11e8-808f-383769e232a3.png)





3. 유저 추가를 쭉 하다보면 접근 키(Access key, Secret key)를 받을 수 있는 페이지가 나옵니다. 

키를 다운받은 다음에 잘 보관해주세요. 이 키들을 서버리스 프레임워크에서 쓸 겁니다.

![alt text](https://user-images.githubusercontent.com/13621579/49515370-11344f80-f8da-11e8-929b-0c184a81c941.png)

키를 받고 나면 iam 페이지에서 나오도록 합시다.



###### Serverless 접근 권한 주기

아까 받은 키들을 서버리스에서 설정해주세요.

```shell
serverlsss login
serverless config --provider aws --key 액세스키 --secret 시크릿액세스키
```



###### serverless 기본 템플릿 만들어보기

서버리스 프레임워크는 템플릿을 써서 기본적인 애플리케이션을 만들 수 있습니다.

```shell
serverless create -t aws-nodejs -p serverless-start
cd serverless-start
```



템플릿을 만들면 파일 2개가 생깁니다. **handler.js**와 **serverless.yml** 입니다.

**```handler.js```** - 람다 실행 코드

```javascript
'use strict';

module.exports.hello = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: 'Go Serverless v1.0! Your function executed successfully!',
      input: event,
    }),
  };

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};
```



**```serverless.yml```** - 서버리스 프레임워크 설정파일 

```yml
service: sls-start 
provider:
  name: aws
  runtime: nodejs8.10
functions:
  hello:
    handler: handler.hello // handler.js에 있는 hello 함수를 실행
```



serverless.yml을 조금 수정하고 배포를 해보겠습니다.



###### serverless.yml 수정 및 배포

**```serverless.yml```** - 서버리스 프레임워크 설정파일 

```yaml
service: sls-start
provider:
  name: aws
  runtime: nodejs8.10
  region: ap-northeast-2
functions:
  hello:
    handler: handler.hello 
    events: // rest api를 만들거니 events는 http로 하겠습니다.
      - http:
          path: hello
          method: get
```



이제 **serverless deploy** 를 터미널에 입력해봅시다. 

성공적으로 배포되면 아래처럼 뜰 거에요.

```shell
endpoints:
  GET - https://kze79anlud.execute-api.ap-northeast-2.amazonaws.com/dev/hello
functions:
  hello: sls-start-dev-hello
```

GET 옆의 url을 브라우저에 붙여넣기해 봅시다.

응답이 나오면 성공이에요!

이제 다이나모 DB와 람다를 연결해보겠습니다.



### DynamoDB 와 람다 연결



다이나모DB를 쓰려면 사용 권한이 있어야해요.

 **```serverless.yml```** 수정해주도록 합시다. 





