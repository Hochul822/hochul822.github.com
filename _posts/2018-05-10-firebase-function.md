---
layout: post
title : [Firebase] cloud function 기본 사용법
date: 2018-05-10 10:00:00
image :
'https://firebasestorage.googleapis.com/v0/b/hellofirebase-373d3.appspot.com/o/tech-blog%2Ffirebase-cloud-function.png?alt=media&token=64d40138-a795-4fe9-b6f2-e332069148da'
category: 'blog'
tags:
- firebase
- serverless
- cloud function
- lambda
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
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
✔  functions[helloWorld]: Successful create operation.
Function URL (helloWorld): https://us-central1-welt-release.cloudfunctions.net/helloWorld
```

URL을 브라우저에 붙여넣어보자.
Hello from Firebase가 나오면 성공이다!
본격적으로 예제를 작성해보자.

##### 가입 유저에게 환영 메일 보내기

#### function 이벤트

#### config 하기

### Promise
