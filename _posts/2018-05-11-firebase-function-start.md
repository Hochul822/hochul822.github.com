---
layout: post
title: "[Firebase] cloud function 기본 사용법"
date: 2018-05-11 08:37:52
image: 'https://firebasestorage.googleapis.com/v0/b/hellofirebase-373d3.appspot.com/o/tech-blog%2Ffirebase-cloud-function.png?alt=media&token=64d40138-a795-4fe9-b6f2-e332069148da'
description: "cloud function의 기본 사용법 및 환영 메일 보내는 법을 배웁니다."
category: 'firebase'
tags:
- app
- firebase
- cloud function
- serverless
twitter_text: twitter text
introduction: hochul instroduction
---
#### Firebase CLI 설치
cloud function은 터미널에서 설정 및 업로드(deploy) 작업을 한다.
터미널 작업을 위해서는 Firebase CLI를 설치해줘야한다.

```shell
npm install -g firebase-tools
```

설치 후 로그인을 해준다.

```shell
firebase login
```

로그인이 끝나면 프로젝트를 설정해주고
원하는 폴더에 firebase project를 만들어 준다.

```shell
firebase init
```

프로젝트 시작시 여러 항목(storage, database, function) 등을 설정해 줄 수 있다.

##### 첫 function 작성하기
프로젝트 설정 후 index.js를 열어보자.
http 호출을 하는 간단한 예제가 있다.

```js
const functions = require('firebase-functions');

exports.helloWorld = functions.https.onRequest((request, response) => {
     response.send("Hello from Firebase!");
});
```

request에 대한 응답으로 'Hello from Firebase!'를 출력하는 함수이다.


##### Cloud function 배포 및 실행

다음 명령어를 통해 함수를 서버에 배포한다.
```shell
firebase deploy --only functions
```

배포가 성공적으로 끝나면 index.js에 있는 함수가 서버에 올라가고
성공 메시지가 뜬다.

```js
functions[helloWorld]: Successful create operation.
Function URL (helloWorld):
 https://us-central1-welt-release.cloudfunctions.net/helloWorld
```

URL을 브라우저에 붙여넣어보자.
Hello from Firebase가 나오면 성공이다!
본격적으로 예제를 작성해보자.

### Trigger 사용
트리거란?
- 다른 파이어베이스의 기능에 이벤트에 발생했을 때, 특정 동작을 수행하도록 하는 것을 트리거라고 한다.
- 예컨데, 사용자가 나이를 바꾼다든지(실시간 데이터베이스 변경), 새로 가입을 했을때, 특정한 일을 하도록 하는 게 트리거다.

이메일 가입시 환영 메일을 보내기 위해서는 인증 트리거(Authentication Trigger)를 사용한다.

### 인증 트리거(Authentication Trigger)

인증 트리거는 2가지 종류가 있다
- 계정 생성시
- 계정 삭제시


1 . 계정 생성시 사용하는 트리거

```js
exports.sendWelcomeEmail = functions.auth.onCreate(event => {
  const user = event.data;
  const email = user.email;
  const displayName = user.displayName;

})
```

위에서는 event.data를 통해 user을 가져왔는데,
바로 user에 접근할 수도 있다.

```js
exports.sendWelcomeEmail = functions.auth.user().onCreate((user) => {
    const email = user.email;
    const displayName = user.displayName;
})
```


2 . 계정 삭제시 사용하는 트리커

```js
exports.sendByeEmail = functions.auth.user().onDelete(event => {

})
```

계정 생성시와 비슷한 방법으로 삭제 트리거를 활용할 수 있다.

### 가입 환영 메일 보내기

가입시 환영 메일을 보내는 전체 코드를 작성해보겠다.

우선 필요한 모듈을 불러오자.


```js
const APP_NAME = "MY_APP";
const functions = require('firebase-functions');
const nodemailer = require('nodemailer'); // node js로 이메일 보내는 모듈
const gmailEmail = functions.config().gmail.email;
const gmailPassword = functions.config().gmail.password;
```

이대로 실행(firebase deploy --only functions)을 하면 error가 나는데, 설정을 제대로 해주지 않아서 그렇다.

terminal로 들어가서 설정을 해주자.

```shell
firebase functions:config:set gmail.email=“hochul9999@gmail.com” gmail.password=“1234”
```

설정 후 다시 실행을 하면 문제없이 실행된다.

nodemailer로 메일을 보내기 전에 어떤 메일로 보낼지 정해야한다.
헌데 gmail

```js

const mailTransport = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: gmailEmail,
    pass: gmailPassword
  },
});

```
###### 헌데 gmail은 권한때문에 바로 실행을 할 수 없다.

https://myaccount.google.com/lesssecureapps?pli=1

여기에 들어가서 **보안 수준이 낮은 앱 허용** 을 눌러주도록 하자

이제 메일을 보내는 부분을 작성하고 함수를 실행해주도록 하자.

```js
exports.sendThisMail = functions.auth.user().onCreate((user) => {
  const email = user.email;
  const displayName = user.email;

  return sendEmail(email, displayName);
})

function sendEmail(email, displayName) {
  const mailOptions = {
    from : `${APP_NAME}@<noreply@gmail.com>`,
    to : email,
  };

  mailOptions.subject = `WELCOME to ${APP_NAME}`;
  mailOptions.text = `Hi ${displayName || ''}! Welcome to ${APP_NAME}`

  return mailTransport.sendMail(mailOptions).then(() => {
      return console.log("Successfully sent email!" + email);
  });
}
```

문제 없이 작업했다면 가입시 환영 메일이 보내질 것이다.

### 전체 코드

```js
const APP_NAME = "MY_APP";
const admin = require('firebase-admin');
const functions = require('firebase-functions');

const nodemailer = require('nodemailer'); // Node 에서 email을 보낼때 사용, gmail에서 보내려면 권한을 낮춰야함
const gmailEmail = functions.config().gmail.email;
const gmailPassword = functions.config().gmail.password;

const mailTransport = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: gmailEmail,
    pass: gmailPassword
  },
});

exports.sendThisMail = functions.auth.user().onCreate((user) => {
  const email = user.email;
  const displayName = user.email;

  return sendEmail(email, displayName);
})

function sendEmail(email, displayName) {
  const mailOptions = {
    from : `${APP_NAME}@<noreply@weltcorp.com>`,
    to : email,
  };

  mailOptions.subject = `WELCOME to ${APP_NAME}`;
  mailOptions.text = `Hi ${displayName || ''}! Welcome to ${APP_NAME}`

  return mailTransport.sendMail(mailOptions).then(() => {
      return console.log("Successfully sent email!" + email);
  });
}
```
