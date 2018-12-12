### 목차

- 1.DynamoDB 설명

- 2.DynamoDB 스키마

- 3.DynamoDB - CRUD, Query, Scan

---

[저번 시간](https://software-creator.tistory.com/14)에는 AWS Lambda와 API Gateway를 쓰는 법을 알아보았는데요.

이번 시간에는 DynamoDB 쓰는 법을 알아보도록 하겠습니다.

모든 튜토리얼이 끝나면 아래 그림과 같은 구조가 될 거에요!



![alt text](https://user-images.githubusercontent.com/13621579/49525336-e3a6d080-f8f0-11e8-90c5-b68b7e5e74a6.png)





### DynamoDB란?

DynamoDB(다이나모디비)는 키-밸류 데이터베이스입니다.

NoSQL방식이라서 확장이 간편하고 사용한만큼만 요금을 내는 구조입니다.

DynamoDB는 스키마를 복잡하게 잡지 않아도 되는 게 장점입니다.

스키마와 상관없이 데이터를 추가가하는 게 가능하기 때문이에요.

DynamoDB는 또한 빠르고 안정성 있게 확장이 가능합니다.

다만 정렬용으로는 좀 맞지 않습니다.



### 1. DynamoDB - 테이블 만들기

#### 1.1 - Table

테이블은 데이터를 담는 그릇입니다.

DynamoDB는 관계형 데이터베이스가 아니기에 조인이 없습니다.

대신 Partition Key(파티션 키)와 Sort Key(소트 키)만 있으면 됩니다.

- Parition Key :  무조건 있어야함. 테이블에 있는 아이템을 어디다가 나눌지 정할 때 쓰임.  Hash Key라고도 불림.

- Sort Key : 정렬할 때 쓰임. 꼭 있어야되는 건 아님. Range Key라고도 불림.


Parition Key와 Sort Key 한번 정해지면은 바꿀 수 없습니다.

이제 테이블을 만들어보고 실제 키들이 어떻게 쓰였는지 보도록 할께요.



#### **1.1.1 - 테이블 예제**

이 테이블은 음식점에서 음식 정보를 담을 때 쓰는 테이블입니다.

음식을 구분하는 id, 종류, 이름, 가격 등이 나와 있죠.

| <u>type</u>         | name       | price | size |
| ------------------- | :--------- | ----- | ---- |
| 양식 (Western Food) | 까르보나라 | 13000 | 소   |
| 한식 (Korean Food)  | 족발       | 33000 | 중   |
| 중식 (Chinese Food) | 짜장면     | 55000 | 대   |

테이블을 만들 때는 Paritition Key(필수)를 정해줘야합니다. Sort Key도 있으면 좋으니 만들도록 할께요.

- Partition Key - **type** 

- Sort key - **name** 

Partition Key의 값들은 가능하면 서로 겹치지 않는게 좋지만

여기선 예제를 단순하기 위해서 음식종류(type)을 썼습니다. 

( 아마존은 **e-mail_id, employee_no, customer_id + product_id** 처럼 중복되는 값이 없는 걸 권장합니다. )



#### 1.2 item(항목)

아이템은 테이블에 있는 각각의 행을 말합니다.

위의 음식 테이블에서는 (1,양식,까르보나라,13000,소)가 아이템이 되겠지요.

아이템에 있는 항목은 키(partition key, sort key)만 뺴고 수정할 수 있습니다.

partition key와 sort key를 수정하려면 지우고 새로 만드는 수 밖에 없습니다.



#### 1.3 데이터 타입

DynamoDB의 데이터는 크게 3가지 타입이 있습니다.

- Scalar Type: Number(숫자, N으로 표시), String(문자, S로 표시), Binary(B로 표시), Boolean and Null
- Document Types: 리스트와 맵
- Set Types: 숫자 집합(Number Set),  문자 집합(String Set), Binary Set(이진자료형 집합)





### 2. DynamoDB 테이블 만들기

#### 2.1. module 설치

이번 예제는 nodejs를 써서 진행하도록 하겠습니다.

일단 aws-sdk를 설치해주세요.

```javascript
npm install -g aws-sdk
```

#### 2.2. aws 설정하기

aws-sdk가 설치되었으면 새로 자바크스립트 파일(**dynamodb_start.js**)을 하나 만들어줍니다.

한국 리전을 설정해주고, 엔드포인트도 변경해줍니다.

```javascript
// dynamodb_start.js
var AWS = require('aws-sdk')

AWS.config.update({
    region: 'ap-northeast-2',
    endpoint: "http://dynamodb.ap-northeast-2.amazonaws.com"
})
```

#### 2.3. 테이블 만들기

이제 데이터베이스 테이블을 만들어볼께요.

일단 테이블을 어떻게 만들지 정합니다.

파티션 키나, 데이터 읽기 쓰기량을 정하는거죠.

지금 만들 테이블은 요리법이 담겨 있는 테이블입니다.

요리종류(type), 이름(name), 재료(ingredients) 등등이 들어있죠.

```javascript
const dynamodb = new AWS.DynamoDB()

const params = {
    TableName: tableName,
    KeySchema: [
        {AttributeName: "type", KeyType: "HASH"},
        {AttributeName: "name", KeyType: "RANGE"}
    ],
    AttributeDefinitions: [
        {AttributeName: "type", AttributeType: "S"},
        {AttributeName: "name", AttributeType: "S"}
    ],
    ProvisionedThroughput: {
        ReadCapacityUnits: 1,
        WriteCapacityUnits: 1
    }
}
```

정해진 설정을 바탕으로 테이블을 만듭니다.

```javascript
dynamodb.createTable(params).promise()
    .then(req => {
        console.log(req)
    })
    .catch(err => {
        console.log(err)
    })
```

제대로 만들어졌나요?

성공하면 아래와 같은 메시지가 나옵니다.

 ```javascript
{ TableDescription:
   { AttributeDefinitions:
      [   { AttributeName: 'type', AttributeType: 'S' },
          { AttributeName: 'name', AttributeType: 'S' } ],
     TableName: 'Food',
     KeySchema:
      [ { AttributeName: 'type', KeyType: 'HASH' },
        { AttributeName: 'name', KeyType: 'RANGE' } ],
     TableStatus: 'CREATING',
     CreationDateTime: 2018-12-05T07:25:06.054Z,
     ProvisionedThroughput:
      { NumberOfDecreasesToday: 0,
        ReadCapacityUnits: 1,
        WriteCapacityUnits: 1 },
     TableSizeBytes: 0,
     ItemCount: 0,
     TableArn: 'arn:aws:dynamodb:ap-northeast-2:xxxxxxxxxxxx:table/Food',
     TableId: 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxx' } }
 ```



이제 테이블에서 데이터(아이템)을 읽고, 쓰고, 업데이트 하는 걸 해보도록 할께요.



### 3. DynamoDB - 데이터 활용하기 (CRUD, QUERY)

#### 3.1 CRUD 작업하기

##### 3.1.1 ADD ITEM

일단 아이템을 테이블에 넣어보도록 할께요.

아이템을 넣는 건 테이블을 만드는 거에 비하면 간단합니다.

아이템의 형태는 자유롭게 정할 수 있습니다만, partition key랑 sort key는 빼먹으면 안됩니다!

```javascript
const docClient = new AWS.DynamoDB.DocumentClient()

const tableName = "Food"
const params = {
        TableName: tableName,
        Item: {
            "type": "Western",
            "name": "Carbonara",
            "price": 13000,
            "ingredients": {
                "egg": "1",
                "spaghetti": "90g",
                "garlic": "1",
                "bacon": "50g"
            }
        }
}

docClient.put(params).promise()
        .then(req => {
            console.log(req)
        })
        .catch(err => {
            console.log(err)
        })
```

잘 되었나요?

3개 정도 아이템을 더 더해보도록 하겠습니다.

이번에는 파일에서 읽을께요.

**```recipes.json```**  - ingredients이 조금씩 달라요.

```json
{
  "items": [
    {
      "type": "Western",
      "name": "Carbonara",
      "price": 13000,
      "ingredients": {
        "egg": "1",
        "spaghetti": "90g",
        "garlic": "1",
        "bacon": "50g"
      }
    },
    {
      "type": "Korean",
      "name": "Spicy chicken stew",
      "price": 25000,
      "ingredients": {
        "chicken": "1",
        "potato": "2",
        "onion": "1",
        "carrot": "1/3",
        "green onion": "2",
        "water": "600ml",
        "sesame seeds": "little",
        "red pepper": "2spoons"
      }
    },
    {
      "type": "Korean",
      "name": "Soybean pastestew",
      "price": 6000,
      "ingredients": {
        "fermented soybean paste": "1T",
        "anchovy broth": "500ml",
        "pumpkin": "1",
        "marbled": "2 pieces",
        "red peper": "1T",
        "bean paste": "0.3 spoon",
        "onion": "1 tablespoon"
      }
    },
    {
      "type": "Western",
      "name": "Cheese cake",
      "price": 13000,
      "ingredients": {
        "honey maid graham cracker crumbs": "1 3/4 cups",
        "butter, melted": "1/3 cup",
        "sugar, divided": "1 1/4 cups",
        "philadelphia cream cheese": "3 packages(8 ounce)",
        "vanilla": "2 teaspoons vanilla",
        "eggs": "3"
      }
    }
  ]
}
```

```javascript
const data = fs.readFileSync("recipes.json")
const recipes = JSON.parse(data)

for(let index in recipes.items) {
    let item = recipes.items[index]

    const params = {
        TableName: tableName,
        Item: item
    }

    docClient.put(params).promise()
        .then(req => {
            console.log(req)
        })
        .catch(err => {
            console.log(err)
        })

}
```



Item의 ingredients가 다르더라도 큰 문제없이 추가가 됩니다.

이런 유연함이 다이나모 디비의 장점이죠.



#### 3.1.2 get item

추가한 아이템을 불러와 볼께요.

여러 아이템을 한꺼번에 불러오려면 다른 방법(query, scan)을 써야되지만

아이템 1개만 가져올땐 get을 쓰면 됩니다.

```javascript
const docClient = new AWS.DynamoDB.DocumentClient()

const params = {
    TableName: "Food",
    Key: {
        "type": "Western",
		"name": "Carbonara"
    }
}

docClient.get(params).promise()
    .then(req => {
        console.log(req)
    })
    .catch(err => {
        console.log(err)
    })
```

get을 쓸땐 꼭 Key를 적어줘야합니다.

잘 되면은 아래처럼 나올거에요.

```json
{ Item:
   { ingredients: { garlic: '1', bacon: '50g', spaghetti: '90g', egg: '1' },
     price: 13000,
     name: 'Carbonara',
     type: 'Western' } }
```



#### 3.1.3 update item

이번엔 아이템의 값을 수정할께요.

```javascript
var docClient = new AWS.DynamoDB.DocumentClient()
var params = {
    TableName: "Food",
    Key: {
        "type": "Western",
        "name": "까르보나라",
    },
    UpdateExpression: "set price = :price", // 어떤 걸 수정할지 정해줘야합니다.
    ExpressionAttributeValues: {  // 수정할 것의 값을 정해줘야합니다.
        ":price": 15000
    }
}

docClient.update(params).promise()
    .then(res => {
        console.log(res)
    })
    .catch(err => {
        console.log(err)
    })
```

**price** 가 바뀌었죠? 

이처럼 일반 값은 얼마든지 수정할 수 있습니다. 

하지만 키(key), 테이블명(table name)은 수정이 안됩니다.

키는 한번 정하면 끝이니 주의하세요.



#### 3.1.4 delete item

아이템을 지워보겠습니다.

단순히 지우는 게 아니라 조건을 넣어보겠습니다.

모든 아이템을 다 지우면 곤란하니까요.

sql에서 **where**를 쓰는 거랑 비슷합니다.

조건은 **ConditionExpression** 을 써서 정합니다.

```javascript
var docClient = new AWS.DynamoDB.DocumentClient()

const params = {
    TableName: tableName,
    Key: {
        "type": "Korean",
        "name": "Soybean pastestew",
    },
    ConditionExpression: "price <= :price",
    ExpressionAttributeValues: {
        ":price": 8000
    }
}

docClient.delete(params).promise()
    .then(res => {
        console.log(res)
    })
    .catch(err => {
        console.log(err)
    })
```

잘 지워졌나요?

지워진 아이템을 가져오려고 하면(get) 아무것도 안 나옵니다.

```javascript
const params = {
    TableName: "Food",
    Key: {
        "type": "Korean",
        "name": "Soybean pastestew"
    }
}

docClient.get(params).promise()
    .then(req => {
        console.log(req)
    })
    .catch(err => {
        console.log(err)
    })
```



지금까지 기본적인 CRUD (Create, Read, Update, Delete)를 알아보았습니다.

이제 데이터를 조회하는 법에서 알아보도록 할께요.



#### 3.2 데이터 조회하기

데이터를 조회하는 방법은 2가지 입니다. 쿼리랑 스캔인데요. 쿼리는 파티션 키를 바탕으로 데이터를 찾습니다. 스캔은 전체 테이블에서 데이터를 찾을 때 쓰구요. 일단 쿼리부터 설명해보겠습니다.

#### 3.2.1 query

쿼리를 잘 쓰려면 파티션 키를 잘 정해야합니다. (아마존에선 e-mail_id, employee_no, customer_id 같이 중복되지 않는 값을 권장합니다.) 파티션 키를 통해 데이터를 분산하고, 조회할 때 쓰기 때문이죠.

위에서 파티션 키를 음식 종류(type)로 정했죠. 음식 종류에 따라 음식을 찾기 위해서 입니다. 한식이 500종류, 일식이 200종류, 양식이 1000종류 있는 경우를 생각해 봅시다. 유저가 50대 한국인이여서 한식만 찾는다면 어떤 쿼리를 써야할까요?

**한식**을 키로 두고 쿼리를 쓰면 되겠죠?

```javascript
const params = {
    TableName: "Food",
    KeyConditionExpression: "#type = :type", // 어떤 키로 조회할지 정합니다.
    ExpressionAttributeNames: { // 조회할 때 쓸 키의 이름, 별명을 정합니다.
        "#type": "type"
    },
    ExpressionAttributeValues: { // 조회할 키의 값을 정합니다.
        ":type": "Korean"
    }
}

docClient.query(params).promise()
        .then(res => {
            res.Items.forEach(item => {
                console.log("name :", item.name, ", type :", item.type)
            })
        })
        .catch(err => {
            console.log(err)
        })
```



쿼리 결과는 아래와 같이 나옵니다. 어렵진 않죠.

다만 다이나모디비의 쿼리는 SQL의 SELECT처럼 자유롭게는 못 쓰고, 파티션 키의 영향을 크게 받는다는 걸 명심하세요.

```shell
name : Soybean pastestew , type : Korean
name : Spicy chicken stew , type : Korean
```



#### 3.2.2 scan

스캔은 전체 테이블에서 데이터를 찾을 때 사용합니다.

스캔은 데이터 양이 많을수록 처리량이 많아서 비효율적이 되죠.

그렇지만 필터를 쓰면 SQL을 쓰는 것처럼 편하게 데이터 조회가 가능한 게 장점입니다.

키를 지정하지 않아도 되는게 쿼리(query)와의 차이점이죠.

```javascript
// scan시 쓰일 함수를 정의합니다.
function onScan(err, data) {
    if (err) {
        console.error("Unable to scan the table. Error JSON: ", JSON.stringify(err, null, 2))
    } else {
        console.log('Scan Succeeded')
        data.Items.forEach(function(food){
            console.log(food)
        })
    }
}


var params = {
   TableName: "Food", // 스캔은 키가 없어도 됩니다.
}

docClient.scan(params, onScan)
```

실행을 하면 전체 데이터를 조회해서 출력합니다.

지금은 데이터가 몇개 안되지만 테이블이 클수록 양이 엄청나겠죠?

전체 데이터를 스캔하지 말고 필터해서 필요한 데이터만 쓰도록 할께요.

```javascript
const params = {
    TableName: "Food",
    ProjectionExpression: "#name,#type,price", // 어떤 속성을 스캔할지 정함.
    FilterExpression: "price between :start_price and :end_price", // 데이터 필터링할 조건을 정함.
    ExpressionAttributeNames: {
        "#name": "name",
        "#type": "type"
    },
    ExpressionAttributeValues: {
        ":start_price": 5000,
        ":end_price": 20000
    }
}

function onScan(err, data) {
    if (err) {
        console.error("Unable to scan the table. Error JSON: ", JSON.stringify(err, null, 2))
    } else {
        console.log('Scan Succeeded')
        data.Items.forEach(function (food) {
            console.log(food)
        })
    }
}

docClient.scan(params, onScan)
```

실행하면 아래처럼 나옵니다.

```json
{ price: 15000, name: '까르보나라', type: 'Western' }
{ price: 6000, name: '차돌박이된장찌개', type: 'Korean' }
```

필터링을 하면 필요한 값만 쓸 수 있습니다.

다만 필터링을 해도 테이블을 다 확인하는 건 동일하다고 하네요.

데이터베이스 입장에서는 필터를 하나, 안 하나 큰 차이가 없는 거지요.

스캔을 적게 써야하는 건 이때문입니다.



### 기타 주의할점 - 테이블명 짓기

테이블이나 인덱스명을 정할 때는 규칙이 있습니다. 3~255자 사이의 글자여야하고, 다음의 문자만 포함할 수 있어요.

- ```a-z```
- `A-Z`
- `0-9`
- `_` (underscore)
- `-` (dash)
- `.` (dot)



자세한 건 [아마존 공식 문서](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.NamingRulesDataTypes.html) 를 참고해주세요.



### 기타 주의할점 - 다이나모 DB를 쓸 때 고려할점

이 [글](https://emptystackdeveloper.wordpress.com/2015/08/16/dynamodb%EB%A5%BC-%EC%84%A0%ED%83%9D%ED%95%98%EA%B3%A0-%EC%8B%B6%EC%9D%80-%EB%8B%B9%EC%8B%A0%EC%97%90%EA%B2%8C-%ED%95%98%EA%B3%A0-%EC%8B%B6%EC%9D%80-%EB%A7%90/)을 읽고 정리해 보았습니다. 공감이 가더군요.

- SQL처럼 쿨하게 조회가 안된다. Key-Value 데이터베이스에 익숙하면 괜찮지만 RDMS에서 바로 왔으면 난감할 수도
- 개발 및 유지 보수가 간단하지 않다. 학습 곡선이 좀 있더군요.
- 해시키 잘 정해야한다. 중간에 못 바꾼다.

장점은 꾸준히 빠르다. 확장이 편리하다 입니다.



### 기타 주의할점 - 스캔 효율적으로 쓰기

아마존의 베스트 프랙티스를 요약해보았습니다.

[Best Practice for Querying and Scanning](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-query-scan.html)

- **Reduce page size** - 스캔을 할 때는 페이지 크기를 줄이기를 권하고 있습니다. 기본값은 1mb인데 Limit 변수를 쓰면 한번에 읽는 페이지수를 조절할 수 있죠.

- Isolate scan operations - 복제 테이블(shadow table)을 만들기를 권합니다. 특정 용도를 위해 테이블을 만들면 스캔때문에 생기는 병목 현상을 막을 수 있습니다. read capacity를 과도하게 쓰는 것도 피할 수 있죠.


### 마무리

오늘은 다이나모 DB에 대해서 알아보았는데요.

다음에는 서버리스 프레임워크 (Serverless Framework)의 사용법과 람다, 다이나모 DB의 연결을 해보도록 하겠습니다.



