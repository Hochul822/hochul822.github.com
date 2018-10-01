- 데이터베이스란?
- shared preference
  - 임시 데이터
- sql database
  - sql helper
- room


### 데이터베이스란?
![alt text](img/db/rdb-simple.png)

많은 데이터를 다루는 거대한 소프트웨어이다.
우리가 문서를 쓰거나, 엑셀을 할때 작게 하나하나의 파일을 만들어서 그 안에 데이터를 저장한다.

파일이 1,2개일때는 문제 없지만 100개, 1000개를 다루고 싶을 때는 어떻게 해야할까? 파일을 고칠 때마다 모든 파일을 업데이트하거나 지워야할까?

파일이 서로 관련 없을 때는 1개의 파일만 쓰면 된다. 반면 여러 파일이 같이 관련 있을때는 어떻게 해야할까? 엑셀을 7,8개씩 띄워놓고 수정할 수도 없는 노릇이다.

이런 문제를 해결하기 위해서 나온 게 데이터베이스이다.

데이터베이스에는 다양한 종류가 있는데, 오라클(한때 세계 1,2위를 다투던 IT회사), mysql, mongodb, sqlite3 등등 무수히 많은 데이터베이스가 있다.

우리는 이 중에서 Sqlite3를 다뤄보도록 하겠다.

### SQLite 3

400kb 정도 밖에 안되는 가벼운 데이터베이스이다.

간단한 데이터를 읽고 쓸 때 쓰인다.

자원이 한정적인 모바일용으로 적합하다.

#### 테이블
SQlite3는 테이블 형태로 데이터를 구분한다.

테이블에는 행과 열이 있고, 첫번째 열에는 어떤 데이터 종류인지 나타내는 값이 들어있다.

![alt text](img/db/table.jpg)

첫번째 행을 보면 name,fname,city,age,salary가 있는데 하나의 데이터를 나타낸다. 이처럼 행은 하나 데이터(여기서는 한사람의 데이터)를 나타낸다.

Name으로 된 열을 보면 모두 이름을 나타내는 것을 알 수 있다. 열은 같은 종류의 데이터를 나타낸다.

그림처럼 전체 데이터의 묶음을 테이블이라고 하며, 테이블 단위로 데이터를 관리(만들고,수정하고,지운다)한다.


##### SQLite Type
- NULL
  - 아무것도 없는 걸 적을때 쓴다.
- INTEGER
  - 정수값. 1,2,3,4,10 등 숫자를 적음
- REAL
  - 실수값. 1.34, 3.56 등 소수점 있는 숫자를 적음
- TEXT
  - 문자열, UTF-8, UTF-16로 인코딩된다.
- BLOB(Binary Large Object)
  - 대형 이진 객체. 작은 텍스트나 이미지, 비디오 등 다양한 형태를 저장한다.

##### SQLite 기본 명령어들

###### 테이블 만들기
```sql
create table employee(
id integer primary key autoincrement,
name text not null,
wage real not null,
age integer
);
```

```sql
create table if not exists employee(
id integer primary key autoincrement,
name text not null,
wage real not null,
age integer
);
```

###### 테이블에 값 넣기 - 모든값
```sql
insert into employee values(1,"영선",200,35);
insert into employee values(2,"지혁",300,30);
insert into employee values(3,"선미",400,29);
insert into employee values(4,"찬혁",250,28);
insert into employee values(5,"정우",1250,21);  
```

###### 테이블에 값 넣기 - 일부만

```sql
insert into employee(name,wage,age) values("미선", 230, 23)
```

###### 테이블에 있는 값 조회하기(query) - 전체
```sql
select * from employee;
```

###### 테이블에 있는 값 조회하기 - 일부
```sql
select * from employee where id = 1;
select name from employee;
```

###### 테이블에 있는 값 변경
```sql
update employee set age = 33 where id = 1;
```

###### 테이블에 있는 값 지우기
```sql
delete from employee where id = 1;
```

###### 테이블 삭제
```sql
drop table employee;
```
##### 각종 부가적인 조건문들
where 문을 통해 조금 더 다양한 조건으로 데이터를 검색할 수 있다.

or : 조건 중 하나라도 만족하는 경우
```sql
select * from employee where id = 1 or name = '선미' or age >= 30;
```

and : 모든 조건을 만족하는 경우

```sql
select * from employee where wage >= 500 and age > 20;
```


like : 일치하는 문자가 있는 경우

like는 %로 일치하는지 여부를 확인한다.
%를 어디에 붙이느냐에 따라 조회 결과가 달라진다.

```sql
select * from employee where wage like '20%'; // 20으로 시작하는 경우
```

```sql
select * from employee where wage like '%0.0'; // 0으로 끝나는 경우
```

```sql
select * from employee where wage like '%200.0%'; // 정확히 200.0이랑 일치하는 경우
select * from employee where name like '정우'; // 정확히 이름이 '정우'인 경우
```


in : or 검색
```sql
select * from employee where age in (20, 28); // age가 20 이거나 28인 경우
select * from employee where age not in (20, 28); // age가 20과 28이 아닌 경우
```

between : 범위 검색
```sql
select * from employee where age between 20 and 28; // age가 20~ 28인경우
```

limit : 조회 결과 숫자 제한
```sql
select * from employee limit 3; // 3개 까지만 나옴.
```


##### Join
여태까지는 테이블을 하나만 사용했지만 실제로는 여러 테이블을 함께 사용하는 게 대부분이다.

일단 테이블을 추가로 만들고 join을 한번 해보도록 하자.

```sql
create table department(
   id integer primary key not null,
   dept char(50) not null,
   emp_id integer not null
);
```
insert 하기

```sql
insert into department (id, dept, emp_id) values(1, 'IT Billing', 1);
insert into department (id, dept, emp_id) values(2, 'Engineering', 2);
insert into department (id, dept, emp_id) values(3, 'Finance', 6);
```

join 하기

```sql
select * from employee outer join department where employee.id = department.emp_id;
select * from employee inner join department where employee.id = department.emp_id;
select * from employee inner join department on employee.id = department.emp_id;
```

### Primary key, Foreign key

테이블끼리 join할때 중요한 게 primary key랑 foreign key이다.

primary key는 각각의 레코드(행)을 구분해 주는 역할을 하며, 무조건 각기 다른 값을 가져야한다.
foreign key는 서로 다른 테이블을 연결할 때 사용한다.

(추가설명할것)


###### 미리 만들어놓은 SQL 파일 읽기

SQL 파일을 따로 정의하고 읽어들이면 된다.

```
sqlite3 shopping.db < init.sql
```


### 안드로이드와의 연동


##### 데이터 베이스 열기

sqlite3를 안드로이드에서 사용하려면 처음에 데이터 베이스를 생성해줘야한다.

이는 **openOrCreateDatabase(데이터베이스 이름, 모드, 팩토리설정)** 란 함수를 통해서 한다.


```java
String dbName = "start.db";
String tableName = "company";
SQLiteDatabase db;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    db = openOrCreateDatabase(dbName, Context.MODE_PRIVATE, null);
}
```
##### Select
```java
void createTable() {
        String sql = "create table if not exists " + tableName + " (id integer primary key autoincrement not null," +
                "name text not null," +
                "address text not null," +
                "age integer not null" +
                ")";

        db.execSQL(sql);
    }

    void insertData(int id, String name, String address, int age) {
        String sql = "insert into " + tableName + " values('" + id +
                "','" + name +
                "','" + address +
                "','" + age +
                "')";
        db.execSQL(sql);
    }




##### Query
INSERT, UPDATE, CREATE는 실행시 **execSQL()** 란 함수를 쓴다.

```java
SQLiteDatabase db;
String sql = "create table store (id integer primary key not null)";
db.execSQL()
```

반면 SELECT의 경우 **rawQuery()** 란 함수를 사용한다.

```java
public void showData() {
    String sql = "select * from " + tableName;
    Cursor cursor = db.rawQuery(sql, null);

    while (cursor.moveToNext()) {
      // 첫 번째 컬럼(Column)이 INTEGER 타입인 경우.
      int val = cursor.getInt(0) ;

      // 두 번째 컬럼(Column)의 타입이 TEXT 인 경우.
      String str = cursor.getString(1) ;

      // 세 번째 컬럼(Column)이 REAL 타입으로 선언된 경우.
      float real = cursor.getFloat(2) ;
    }
}
```   
