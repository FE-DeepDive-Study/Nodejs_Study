
# 5장 자바스크립트에서 비동기 처리하기

## 5.1 자바스크립트 비동기 소개

- 동기 프로그래밍은 작업이 순차적으로 실행되며, 작업이 완료될 때까지 중단될 수 없음.
- 비동기 프로그래밍은 임의의 순서로 또는 동시에 작업이 실행될 수 있음.
- 자바스크립트는 싱글 스레드로 동작하지만, 콜백, 프로미스, async/await 방법을 사용해 비동기 처리 가능.
    - 콜백: 함수 전달. 가독성 나쁨
    - Promise: then, catch 메서드를 통해 성공, 실패 처리
    - async/await: 비동기 작업을 동기적으로 처리하는 것처럼 코드를 작성할 수 있게 해줌

## 5.2 콜백 함수 소개

- 콜백은 비동기 작업이 완료되면 호출되는 함수.
- 콜백의 문제점은 가독성이 좋지 않고, 중첩된 콜백으로 인해 "콜백 지옥"이 발생할 수 있음.

## 5.3 Promise 객체

- 콜백의 단점을 보완하기 위해 ES6에서 도입된 객체.
- 비동기 작업의 성공 또는 실패를 나타내는 객체로, 다음 세 가지 상태를 가짐:
  - **대기(pending)**: 이행하거나 거절되지 않은 초기 상태.
  - **이행(fulfilled)**: 연산이 성공적으로 완료된 상태.
  - **거절(rejected)**: 연산이 실패한 상태.
- .then(), .catch(), .finally() 메서드를 통해 비동기 작업의 결과를 처리할 수 있음.

### 프로미스의 기본 사용법

1. 프로미스 생성:
   ```javascript
   const promise = new Promise((resolve, reject) => {
       // 비동기 작업 수행
       if (성공) {
           resolve(결과); // 성공 시 결과 반환
       } else {
           reject(오류); // 실패 시 오류 반환
       }
   });
   ```

2. 프로미스 사용:
   ```javascript
   promise
       .then(result => {
           // 성공 처리
       })
       .catch(error => {
           // 실패 처리
       })
       .finally(() => {
           // 항상 실행
       });
   ```

### 프로미스 체이닝

- 여러 비동기 작업을 순차적으로 처리할 때 프로미스를 체인(chain)으로 연결할 수 있음.
- 각 .then() 블록은 이전 블록의 결과를 받아 다음 작업을 수행함.

  ```javascript
  firstFunction()
      .then(result => secondFunction(result))
      .then(result => thirdFunction(result))
      .catch(error => console.error(error));
  ```

- 장점:
  - 가독성이 높아지고 코드의 흐름을 이해하기 쉬움.
  - 에러 처리를 중앙 집중화할 수 있음.

### 프로미스의 단점

- 여전히 가독성이 떨어지고, 복잡한 비동기 로직을 처리하기 어려움.
- 이러한 단점을 보완하기 위해 async/await 문법이 도입됨.
## 5.4 async와 await

- async와 await는 자바스크립트에 가장 최근 도입된 비동기 처리 방식.
- 기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하여 가독성 높은 코드를 작성할 수 있음.
- **async** 키워드는 함수 앞에 붙여서 해당 함수가 비동기 함수임을 명시. 이 함수는 항상 프로미스를 반환함.
- **await** 키워드는 프로미스가 처리될 때까지 기다림. async 함수 내에서만 사용 가능.

### async와 await의 기본 사용법

1. async 함수 정의:
   ```javascript
   async function myName() {
       return "Andy";
   }
   console.log(myName());
   ```
   - 이 코드는 Promise를 반환하며, 결과 값으로 `Promise { 'Andy' }`를 출력함.

2. await 키워드 사용:
   ```javascript
   async function showName() {
       const name = await myName();
       console.log(name);
   }
   showName();
   ```
   - 이 코드는 `Andy`를 출력함. await은 myName() 함수의 실행이 끝날 때까지 기다림.

### async와 await를 사용한 예제

- 1초마다 메시지를 출력하는 예제:
  ```javascript
  function waitOneSecond(msg) {
      return new Promise((resolve) => {
          setTimeout(() => resolve(`${msg}`), 1000);
      });
  }

  async function countOneToTen() {
      for (let x of [...Array(3).keys()]) {
          let result = await waitOneSecond(`${x + 1} 초 대기 중...`);
          console.log(result);
      }
      console.log("완료");
  }

  countOneToTen();
  ```

- 이 코드는 1부터 3까지 1초마다 메시지를 출력하고 마지막에 "완료"를 출력함.

### 프로미스와 async/await 비교

| 구분      | callback                       | promise              | async/await            |
|-----------|--------------------------------|----------------------|------------------------|
| 에러 처리 | 콜백 함수 내에서 처리         | catch() 메서드로 처리 | try-catch 블록으로 처리 |
| 가독성    | 간단한 경우에는 괜찮으나, 점점 복잡해짐 | 가독성 좋음           | 가독성 좋음             |
| 중첩 처리 | 콜백 함수 내에서 처리          | then() 메서드를 사용  | await 키워드를 사용     |

# 6장 NoSQL 데이터베이스 몽고디비 사용하기

## 6.1 몽고디비 소개

- **NoSQL 데이터베이스**: 관계형 데이터베이스와 달리 테이블 구조를 따르지 않는 데이터베이스.
- **몽고디비(MongoDB)**: 도큐먼트(Document) 지향 NoSQL 데이터베이스로, JSON과 유사한 BSON 형식의 데이터를 저장함.
- **주요 특징**:
  - 스키마가 없어 유연성 제공.
  - 수평 확장성 지원.
  - 높은 성능과 다양한 쿼리 기능 제공.
- **기본 용어**:
  - **도큐먼트(Document)**: BSON 형식의 데이터 단위.
  - **컬렉션(Collection)**: 도큐먼트의 집합.
  - **데이터베이스(Database)**: 컬렉션의 집합.
  - **클러스터(Cluster)**: 여러 서버에 분산된 데이터베이스.

### 6.1.1 데이터베이스 기본 용어
![](attachment/be3d1e9de6a4194f08d8fc6e7d324d9e.png)

- **테이블**: 특정 주제에 대한 행과 열로 이루어진 데이터의 모음.
- **행(Row)**: 단일 구조 데이터 항목.
- **열(Column)**: 특정한 자료의 값.
- **기본키(Primary Key, PK)**: 중복된 값을 가질 수 없는 데이터 식별 키.
- **외래키(Foreign Key)**: 두 테이블을 연결하는 키.
- **스키마(Schema)**: 데이터베이스 테이블의 명세를 기술한 데이터.
- **컬렉션(collection)**: 도큐먼트의 집합 (=RDB의 테이블)
- **트랜잭션(Transaction)**: 데이터 변경을 수행하는 작업 단위

### 6.1.2 몽고디비 특징

- **BSON**: Binary JSON의 약자로, JSON을 바이너리 형식으로 저장.
- **장점**:
  - 스키마 지정이 필요 없으므로 데이터 저장의 유연성 제공.
  - 단일 문서 검색 시 빠른 성능.
  - 클러스터를 지원을 통한 쉬운 스케일 아웃 지원.
  - 다른 NoSQL 대비 다양한 인덱스 지원.
- **단점**:
  - 메모리와 디스크 사용량이 많음.
  - 복잡한 조인과 트랜잭션 지원이 약함.

## 6.2 몽고디비 아틀라스 설정하기

- **몽고디비 아틀라스**: 클라우드 기반의 몽고디비 관리 서비스.
    - https://www.mongodb.com/atlas
- **설치 및 설정**:
  1. MongoDB Atlas 계정 생성 및 로그인.
  2. 클러스터 생성 및 설정.
  3. 데이터베이스 사용자 및 네트워크 접근 설정.

```js
const { MongoClient, ServerApiVersion } = require('mongodb');

const uri = `mongodb+srv://${id}:${pw}@cluster0.a8gl9yu.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0`;

// Create a MongoClient with a MongoClientOptions object to set the Stable API version
const client = new MongoClient(uri, {
    serverApi: {
        version: ServerApiVersion.v1,
        strict: true,
        deprecationErrors: true,
    }
});

async function run() {
    // Connect the client to the server	(optional starting in v4.7)
    await client.connect();
    // Send a ping to confirm a successful connection
    await client.db("admin").command({ ping: 1 });
    console.log("Pinged your deployment. You successfully connected to MongoDB!");

    const adminDB = client.db('test').admin();
    const listdatabases = await adminDB.listDatabases();
    console.log(listdatabases);
    return "OK";
}

run().then(console.log)
    .catch(console.error)
    .finally(() => client.close());
```

## 6.3 몽고디비 CRUD API 만들기

- **CRUD**: Create, Read, Update, Delete의 약자로, 데이터 조작 기본 연산.

```js
const MongoClient = require('mongodb').MongoClient;

const url = `mongodb+srv://${id}:${pw}@cluster0.a8gl9yu.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0`;

// 본인의 정보를 기입하세요. 
// myFirstDatabase에서 test로 데이터베이스 이름을 변경합니다.

// 1️⃣ MongoClient 생성 
const client = new MongoClient(url, { useNewUrlParser: true });

async function main() { 
    try { 
        // 2️⃣ 커넥션을 생성하고 연결 시도 
        await client.connect(); 
        console.log('MongoDB 접속 성공');

        // 3️⃣ test 데이터베이스의 person 컬렉션 가져오기
        const collection = client.db('test').collection('person');

        // 4️⃣ 문서 하나 추가
        await collection.insertOne({ name: 'Andy', age: 30 });
        console.log('문서 추가 완료');

        // 5️⃣ 문서 찾기
        const documents = await collection.find({ name: 'Andy' }).toArray();
        console.log('찾은 문서:', documents);

        // 6️⃣ 문서 갱신하기
        await collection.updateOne({ name: 'Andy' }, { $set: { age: 31 } });
        console.log('문서 업데이트');

        // 7️⃣ 갱신된 문서 확인하기
        const updatedDocuments = await collection.find({ name: 'Andy' }).toArray();
        console.log('갱신된 문서:', updatedDocuments);

        // 8️⃣ 문서 삭제하기
        // await collection.deleteOne({ name: 'Andy' });
        // console.log('문서 삭제');

        // 연결 끊기
        await client.close(); 
    } catch (err) { 
        console.error(err); 
    } 
}

main();
```

`$set` 은 몽고디비의 연산자이다. 

자주 사용하는 연산자 모음

![](attachment/523504f0c291c324770b4de278b1956d.png)
## 6.4 몽고디비 컴퍼스로 데이터 확인하기

![](attachment/874885bf8886d157455c9e50254a0045.png)

- **MongoDB Compass**: GUI 기반의 몽고디비 관리 툴.
- **기능**:
  - 데이터베이스와 컬렉션을 시각적으로 관리.
  - 데이터 조회, 삽입, 수정, 삭제 기능 제공.

## 6.5 몽구스를 사용해 CRUD 만들기

- **몽구스(Mongoose)**: Node.js와 몽고디비를 쉽게 연동하기 위한 ODM(Object Data Modeling) 라이브러리.
- **설치**:
  ```bash
  npm install mongoose
  ```
- **기본 사용법**:
  - 스키마 정의, 모델 생성, CRUD 연산 수행.
  - 예제:
    ```javascript
    const mongoose = require('mongoose');

    mongoose.connect('mongodb://localhost/test', { useNewUrlParser: true, useUnifiedTopology: true });

    const userSchema = new mongoose.Schema({
        name: String,
        age: Number
    });

    const User = mongoose.model('User', userSchema);

    const user = new User({ name: 'John', age: 30 });
    user.save().then(() => console.log('User saved'));
    ```

### 6.5.1 몽구스 설치하기

- 몽구스를 사용하려면 npm을 통해 설치 필요.
  ```bash
  npm install mongoose
  ```

### 6.5.2 몽구스로 스키마 만들기

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const personSchema = new Schema({
    name: String,
    age: Number,
    email: { type: String, required: true },
});

module.exports = mongoose.model('Person', personSchema);
```

![](attachment/031fb6a7499450b42e38e3f0fd2860b4.png)
![](attachment/fa2488f128086093227824e876482718.png)

![](attachment/94fc14d3d8f13ce516483540dd9a1755.png)
![](attachment/b7cfe9ca419910416cba82db07bbf2fd.png)
### 6.5.3 몽구스와 익스프레스로 CRUD API 만들기

- 몽구스와 익스프레스를 사용하여 CRUD API를 구현.
  ```javascript
const express = require("express");
const bodyParser = require("body-parser");
const mongoose = require("mongoose");
const Person = require("./person-model");

const secret = require('./../../secret.js')
const uri = `mongodb+srv://${secret.id}:${secret.pw}@cluster0.a8gl9yu.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0`;

mongoose.set("strictQuery", false); // ① 설정해줘야 경고가 뜨지 않음

const app = express();
app.use(bodyParser.json()); // ② HTTP에서 Body를 파싱하기 위한 설정
app.listen(3000, async () => {
  console.log("Server started");
  const mongodbUri = uri; // ③ 몽고디비에 커넥션 맺기
  mongoose
    .connect(mongodbUri, { useNewUrlParser: true })
    .then(console.log("Connected to MongoDB"));
});

// ④ 모든 person 데이터 출력
app.get("/person", async (req, res) => {
  const person = await Person.find({});
  res.send(person);
});

// ⑤ 특정 이메일로 person 찾기
app.get("/person/:email", async (req, res) => {
  const person = await Person.findOne({ email: req.params.email });
  res.send(person);
});

// ⑥ person 데이터 추가하기
app.post("/person", async (req, res) => {
  const person = new Person(req.body);
  await person.save();
  res.send(person);
});

// ⑦ person 데이터 수정하기
app.put("/person/:email", async (req, res) => {
    console.log(req.body)
  const person = await Person.findOneAndUpdate(
    { email: req.params.email },
    { $set: req.body },
    { new: true }
  );
  console.log(person);
  res.send(person);
});

// ⑧ person 데이터 삭제하기
app.delete("/person/:email", async (req, res) => {
  await Person.deleteMany({ email: req.params.email });
  res.send({ success: true });
});


  ```

## 6.6 REST 클라이언트로 API 테스트하기

- **REST 클라이언트**: HTTP 요청을 테스트하기 위한 도구.
- **예제**: VSCode의 REST Client 플러그인.

```
# ① server 변수 설정
@server = http://localhost:3000

### ② GET 요청 보내기
GET {{server}}/person

### ③ POST 요청 보내기
POST {{server}}/person
Content-Type: application/json

{
  "name": "Andy Park",
  "age": 30,
  "email": "andy@backend.com"
}

### ④ 생성한 문서 확인
GET {{server}}/person/andy@backend.com

### ⑤ PUT 요청 보내기, 문서 수정하기
PUT {{server}}/person/andy@backend.com
Content-Type: application/json

{
  "age": 32
}

### 문서 삭제하기
DELETE {{server}}/person/andy@backend.com
```

주의!
```
Content-Type: application/json
        <---- 이 공백을 통해 header와 body를 구분
{
  "age": 32
}
```

# 7장 페이지네이션이 되는 게시판 만들기

![](attachment/c39430eb15bf7cf050c78221c946b93c.png)
## 7.1 프로젝트 구조 소개
![](attachment/d28476a69854cf98a078daeb2caa3d4e.png)

- N 계층 아키텍처 따름
    - MVC 패턴
        - View: 화면
            - HTML 템플릿 엔진: 핸들바(handlebars)로 작성
        - Controller: app.js 라우터 함수
        - Service: post-service.js
            - 구조상 model에서 db와 통신해야 함.
            - 다만 명시적으로 모델 안 만들고 몽고디비 라이브러리의 컬렉션 객체가 역할 

## 7.2 게시판 프로젝트 셋업
### 7.2.1 Node.js 프로젝트 초기 설정
- 노드 프로젝트 생성 및 초기

```
npm init -y
```

### 7.2.2 익스프레스 설치 및 프로젝트 디렉터리 구조 잡기


- 익스프레스 설치
  ```bash
  $ npm i express@4.17.3
  ```
  
- 몽고디비 설치
  ```bash
  $ npm i mongodb@4.13.0
  ```
  
- `package.json` 확인
  ```json
  {
    "name": "board",
    "version": "1.0.0",
    "dependencies": {
      "express": "4.17.3",
      "mongodb": "4.13.0"
    }
  }
  ```

- 익스프레스는 마이크로 프레임워크
    - 최소 기능을 가지고 있으며 필요한 기능은 플러그인으로 확장 가능함.
- 디렉터리 구조는 MVC 패턴을 적용하여 구성함.
    - ![](attachment/22ffe9ee705e5b274c32b585c59be452.png)
    - 컨트롤러: 익스프레스 라우터
        - 요청을 받아 권한 체크, 유효성 검증 후 서비스 계층으로 넘김
    - 서비스: 서비스 클래스 (함수)
        - 비즈니스 로직을 처리하고 데이터 액세스 계층으로 넘김
    - 데이터 액세스: 몽고디비 모듈

![](attachment/4bf40babebce06a0d645c75637c648a2.png)

```plantuml
@startuml
actor 웹페이지
participant "express-router" as 컨트롤러
participant 서비스
participant "몽고디비 모델" as 모델

웹페이지 -> 컨트롤러 : 요청(request) 전송
컨트롤러 -> 서비스 : 인증, 유효성 검증 후 함수 호출
서비스 -> 모델 : 모델 객체 생성 및 데이터 전달 (CRUD 수행)
모델 --> 서비스 : 데이터 처리 결과 반환
서비스 --> 컨트롤러 : 데이터 처리 결과 반환
컨트롤러 --> 웹페이지 : 응답(response) 전송
웹페이지 -> 웹페이지 : 웹페이지 값 확인 및 갱신

@enduml

```

- 프로젝트 디렉터리 구조 설정

![](attachment/07c7bea656da3b3c774ed55139eb1439.png)




### 7.2.3 핸들바 템플릿 엔진 설치 및 설정하기
- 템플릿 엔진 쓸거임
    - Pug(HTML 아님), EJS(표현식 지저분), mustache(기능 적음)
    - Handlebars(mustache와 호환되면서 추가 기능)
    - => Handlebars 로 결정

```
npm i express-handlebars@6.0.3 # 핸들바 설치
```

`./views/home.handlebars`
```html
<h2>{{title}}</h2>
<p>{{message}}</p>
```

`./app.js`
```js
const express = require("express");
const handlebars = require("express-handlebars");
const app = express();

app.engine("handlebars", handlebars.engine()); // ① 템플릿 엔진으로 핸들바 등록
app.set("view engine", "handlebars"); // ② 웹페이지 로드 시 사용할 템플릿 엔진 설정
app.set("views", __dirname + "\\views"); // ③ 뷰 디렉터리를 views로 설정

// ④ 라우터 설정
app.get("/", (req, res) => {
  res.render("home", { title: "안녕하세요", message: "만나서 반갑습니다!" });
});

app.listen(3000);

```

![](attachment/6e0f7c0bdfc30238bba7b72068699a7d.png)
## 7.3 화면 기획하기
- 게시판 기능 고려사항: 글쓰기, 리스트, 댓글, 이미지 업로드, 등록일 표시, 조회수 카운트, 페이징

### 7.3.1 리스트 화면 기획

![](attachment/8c8c8c87d727b3bed3bb3e11f0785aaa.png)

- **게시판 제목**: 페이지 상단에 표시
- **검색 기능**: 제목과 작성자를 검색할 수 있는 입력 필드와 검색 버튼
- **글쓰기 버튼**: 새로운 글을 작성할 수 있는 페이지로 이동
- **제목**: 100자까지 입력 가능
- **작성자 항목**: 글쓰기 페이지에 필요
- **조회수 증가**: 게시글 클릭 시 조회수 증가
- **등록일 형식**: yyyy.mm.dd 형태로 표시
- **페이징 기능**: 한 페이지에 10개의 게시글, 페이지 이동 링크 (예: «, », <, >)

### 7.3.2 글쓰기 화면 기획
![](attachment/f1a03a0e61935f8074e6df9e8b07fa9a.png)
1. 화면 상단 제목
2. 제목: 텍스트 입력 필드
3. 이름: 텍스트 입력 필드
4. 비밀번호: 패스워드 입력 필드
5. 내용: 텍스트 영역
6. 버튼
    1. 저장: 글 저장 후 목록 페이지로 이동
    2. 취소: 목록 페이지로 이동

### 7.3.3 상세 화면 기획
![](attachment/c7bf8d58b605cdf5553a69f6352c3666.png)

1. 게시글 제목
2. 작성자 명, 조회수, 작성일 정보.
    1. 삭제 버튼 클릭 시 비밀번호를 작성자에게 입력하도록 하여 확인 후 삭제 (댓글 있으면 삭제 불가)
3. 게시글 내용
4. 게시글과 댓글 구분 영역
    1. `{{N}} 개의 댓글이 있습니다` 를 표시
5. 댓글 개수만큼 표시 (페이징 없음)
    1. 인증 없으므로 댓글 남길 때 비밀번호 입력
    2. 삭제 클릭시 비밀번호 맞을 경우 삭제
6. 댓글을 추가하기 위한 폼.
    1. 이름 비밀번호 내용 넣고 작성 버튼을 클릭시 추가
7. 목록 버튼을 누르면 게시글 목록 1페이지로 이동

## 7.4 UI 화면 만들기
- UI를 먼저 만들고 기능 추가
- 프론트라 스킵

## 7.5 게시판 API 만들기

게시판 UI 화면을 만들었으니 이제 백엔드 API를 작성합니다. 인증 기능을 생략하고 게시글마다 비밀번호를 넣어서 수정/삭제를 하도록 만들겠습니다. API는 글쓰기, 리스트, 상세 페이지, 글 수정, 삭제, 댓글 추가, 삭제 순서로 작성합니다. 모든 API는 `async/await`를 사용하여 구현합니다.

### 7.5.1 몽고디비 연결을 위한 유틸리티 만들기

- 데이터베이스 연결을 위해 몽고디비 커넥션을 반환하는 유틸리티 함수를 작성함
- `mongodb-connection.js` 파일을 생성하고 아이디, 패스워드, 아틀라스 서버 주소를 적절하게 변경하여 사용
### 7.5.2 UI 페이지에서 사용할 핸들바 커스텀 헬퍼 만들기

- 핸들바 템플릿에서 사용할 커스텀 헬퍼를 정의함
- `helpers.js` 파일에 커스텀 헬퍼 함수를 작성하고 이를 등록하여 템플릿 엔진에 통합

```js
module.exports = {
  // ❶ 리스트 길이를 반환
  lengthOfList: (list = []) => list.length,
  // ❷ 두 값을 비교해 같은지 유무를 반환
  eq: (val1, val2) => val1 === val2,
  // ❸ ISO 날짜 문자열에서 날짜만 반환
  dateString: (isoString) => new Date(isoString).toLocaleDateString(),
};

```

`{{헬퍼함수1 (헬퍼함수2 변수1 변수2) 변수2 변수3 ... 변수5}}` 와 같이 사용

`app.js`
```js
app.engine(
  "handlebars", 
  handlebars.create({
    helpers: require("./configs/handlebars-helpers"),
  }).engine,
); 
```

핸들바 커스텀 함수 추가

### 7.5.3 nodemon 설정하기

- 개발 중 코드 변경 시 서버를 자동으로 재시작하도록 `nodemon`을 설정함
- `nodemon.json` 파일을 생성하여 `watch` 디렉토리를 지정하고, `ignore` 디렉토리를 설정함

- 코드 저장 시 서버 재시작 유틸 설치 (nodemon)
- npm start 명령어 추가 

```json
{
  "name": "board",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "npx nodemon app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "express": "^4.17.3",
    "express-handlebars": "^6.0.3",
    "mongodb": "^4.13.0",
    "nodemon": "^2.0.20"
  }
}

```

### 7.5.4 글쓰기 API 만들기

- 글쓰기 기능을 제공하는 API를 작성함
- 게시글 데이터를 받아 몽고디비에 저장하고 결과를 반환하는 엔드포인트를 구현함

- 게시글 로딩, 저장, 수정, 삭제 로직은 서비스 디렉터리 아래에 담는 게 좋다.

```js
app.post("/write", async(req, res) => {
  const post = req.body;
  const result = await postService.writePost(collection, post);
  res.redirect(`/detail/${result.insertedId}`);
})
```

  ```javascript
async function writePost(collection, post) {
  post.hits = 0;
  post.createdDt = new Date().toISOString();
  return await collection.insertOne(post);
}

module.exports = {
  writePost,
}
  ```

![](attachment/e980aa215dade97acb92285d076b1b6a.png)

### 7.5.5 리스트 API 만들기

- 게시글 목록을 제공하는 API를 작성함
- 페이징 기능을 추가하여 특정 페이지의 게시글 목록을 반환하는 엔드포인트를 구현함

- 검색과 페이지네이션 고려 필요

```html
<input type="text" name="search" id="search" value="{{search}}" size="50" placeholder="검색어를 입력하세요." />
<button onclick="location.href=`/?search=${document.getElementById('search').value}`">검색</button>
```

```js
app.get("/", async (req, res) => {
  const page = parseInt(req.query.page) || 1; // 현재 페이지 데이터
  const search = req.query.search || ""; // 검색어 데이터
  try {
    // postService.list에서 글리스트와 페이지네이터를 가져옴
    const [posts, paginator] = await postService.list(collection, page, search);

    // 리스트 페이지 렌더링
    res.render("home", { title: "테스트 게시판", search, paginator, posts });
  } catch (error) {
    console.error(error);
    res.render("home", { title: "테스트 게시판" }); // 에러가 나는 경우는 빈값으로 렌더링
  }
});
```
`리스트 렌더링 추가`

```js
const paginator = require("../utils/paginator");

async function writePost(collection, post) {
  post.hits = 0;
  post.createdDt = new Date().toISOString();
  return await collection.insertOne(post);
}

async function list(collection, page, search) {
  const perPage = 10;
  const query = { title: new RegExp(search, "i") };
  const cursor = collection
    .find(query, { limit: perPage, skip: (page - 1) * perPage })
    .sort({
      createdDt: -1,
    });
  const totalCount = await collection.count(query);
  const posts = await cursor.toArray();
  const paginatorObj = paginator({ totalCount, page, perPage: perPage });
  return [posts, paginatorObj];
}

module.exports = {
  list,
  writePost,
}
```
`list` 추가

```js
const lodash = require("lodash"); // ❶
const PAGE_LIST_SIZE = 10; // ❷

module.exports = ({ totalCount, page, perPage = 10 }) => {
  // ❸
  const PER_PAGE = perPage;
  const totalPage = Math.ceil(totalCount / PER_PAGE); // ❹

  // 시작 페이지 : 몫 * PAGE_LIST_SIZE + 1
  let quotient = parseInt(page / PAGE_LIST_SIZE);
  if (page % PAGE_LIST_SIZE === 0) {
    quotient -= 1;
  }
  const startPage = quotient * PAGE_LIST_SIZE + 1; // ❺

  // 끝 페이지 : startPage + PAGE_LIST_SIZE - 1
  const endPage =
    startPage + PAGE_LIST_SIZE - 1 < totalPage
      ? startPage + PAGE_LIST_SIZE - 1
      : totalPage; // ❻
  const isFirstPage = page === 1;
  const isLastPage = page === totalPage;
  const hasPrev = page > 1;
  const hasNext = page < totalPage;

  const paginator = {
    pageList: lodash.range(startPage, endPage + 1), // ❼
    page,
    prevPage: page - 1,
    nextPage: page + 1,
    startPage,
    lastPage: totalPage,
    hasPrev,
    hasNext,
    isFirstPage,
    isLastPage,
  };
  return paginator;
};

```
`페이지네이션 로직`


![](attachment/49ac78d7948dd737be34b241d71b4693.png)

### 7.5.6 상세페이지 API 만들기

- 특정 게시글의 상세 정보를 제공하는 API를 작성함
- 게시글 ID를 받아 해당 게시글의 상세 정보를 반환하는 엔드포인트를 구현함

```js
app.get("/detail/:id", async(req, res) => {
  const result = await postService.getDetailPost(collection, req.params.id);
  res.render("detail", {
    title: "테스트 게시판",
    post: result.value,
  })
});
```

```js
const { ObjectId } = require("mongodb");

const projectionOption = {
  projection: {
    // 프로젝션(투영) 결괏값에서 일부만 가져올 때 사용
    password: 0, // 빼고 가져온다
    "comments.password": 0,
  },
};

async function getDetailPost(collection, id) {
  return await collection.findOneAndUpdate(
    { _id: ObjectId(id) },
    { $inc: { hits: 1 } },
    projectionOption
  );
}
```

## 7.5.7 글수정 API 만들기

- 게시글을 수정하는 기능을 제공하는 API를 작성함
- 수정할 게시글 ID와 수정 데이터를 받아 업데이트를 수행하고 결과를 반환하는 엔드포인트를 구현함

```js
  const postOption = {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
  }

  async function modifyPost() {
    const password = prompt("패스워드를 입력해주세요");
    // ❶ 프롬프트에서 취소를 누른 경우 처리 
    if (!password) {
      return;
    }

    // ❷ check-password API 실행 
    const result = await fetch("/check-password", {
      ...postOption,
      body: JSON.stringify({ id: "{{post._id}}", password })
    });

    // ❸ json 함수를 실행하는 경우도 await를 해줘야 함 
    const data = await result.json();

    // ❹ 패스워드가 맞는 경우 수정 페이지로 이동 
    if (data.isExist) {
      document.location = "/modify/{{post._id}}"
    } else {
      alert("패스워드가 올바르지 않습니다.");
    }
  }
```

`view` 업데이트


```js
app.post("/check-password", async (req, res) => {
  // id, password 값을 가져옴
  const { id, password } = req.body;

  // postService의 getPostByIdAndPassword() 함수를 사용해 게시글 데이터 확인
  const post = postService.getPostByIdAndPassword(collection, { id, password });

  // 데이터가 있으면 isExist true, 없으면 isExist false
  if (!post) {
    return res.status(404).json({ isExist: false });
  } else {
    return res.json({ isExist: true });
  }
});
```

`controller` 업데이트

```js
async function getPostByIdAndPassword(collection, { id, password }) {
  //  ❶ findOne() 함수 사용
  return await collection.findOne(
    { _id: ObjectId(id), password: password },
    projectionOption
  );
}

// ❷ id로 데이터 불러오기
async function getPostById(collection, id) {
  return await collection.findOne({ _id: ObjectId(id) }, projectionOption);
}

// ❸ 게시글 수정
async function updatePost(collection, id, post) {
  const toUpdatePost = {
    $set: {
      ...post,
    },
  };
  return await collection.updateOne({ _id: ObjectId(id) }, toUpdatePost);
}
```

`service` 업데이트

```js
// 수정 페이지로 이동 mode는 modify
app.get("/modify/:id", async (req, res) => {
  const { id } = req.params.id;
  // getPostById()  함수로 게시글 데이터를 받아옴
  const post = await postService.getPostById(collection, req.params.id);
  console.log(post);
  res.render("write", { title: "테스트 게시판 ", mode: "modify", post });
});

// 게시글 수정 API
app.post("/modify/", async (req, res) => {
  const { id, title, writer, password, content } = req.body;

  const post = {
    title,
    writer,
    password,
    content,
    createdDt: new Date().toISOString(),
  };
  // 업데이트 결과
  const result = postService.updatePost(collection, id, post);
  res.redirect(`/detail/${id}`);
});
```

`controller` 업데이트

![](attachment/872d917616b161d4166cb45c470450d0.png)
![](attachment/173163942a469bc58c6a739ffeda35c3.png)

수정하면 등록일도 바뀜 
## 7.5.8 글삭제 API 만들기

- 게시글을 삭제하는 기능을 제공하는 API를 작성함
- 삭제할 게시글 ID를 받아 삭제를 수행하고 결과를 반환하는 엔드포인트를 구현함

```js
const deleteOption = {
    method: 'DELETE',
    headers: {
      'Content-Type': 'application/json',
    },
  }

  async function deletePost() {
    const password = prompt("삭제하려면 패스워드를 입력해주세요"); // ❶ 프롬프트로 값 입력받기
    // 프롬프트에서 취소를 누른 경우 처리 
    if (!password) {
      return;
    }
    const result = await fetch("/delete", {   // ❷ fetch API를 사용해 delete API를 호출 
      ...deleteOption,
      body: JSON.stringify({ id: "{{post._id}}", password })
    });

    // ❸ delete API의 결과에 따라 다른 메시지 출력
    const data = await result.json();
    if (!data.isSuccess) {
      alert("삭제에 실패했습니다. 패스워드를 확인해주세요.");
      return;
    }

    document.location = "/";
  }

```
`view`

```js
app.delete("/delete", async (req, res) => {
  const { id, password } = req.body;
  try {
    // collection의 deleteOne을 사용해 게시글 하나를 삭제
    const result = await collection.deleteOne({
      _id: ObjectId(id),
      password: password,
    });
    // 삭제 결과가 잘 못된 경우의 처리
    if (result.deletedCount !== 1) {
      console.log("삭제 실패");
      return res.json({ isSuccess: false });
    }
    return res.json({ isSuccess: true });
  } catch (error) {
    // 에러가 난 경우의 처리
    console.error(error);
    return res.json({ isSuccess: false });
  }
});
```
`controller`


## 7.5.9 댓글추가 API

- 게시글에 댓글을 추가하는 기능을 제공하는 API를 작성함
- 댓글 데이터를 받아 몽고디비에 저장하고 결과를 반환하는 엔드포인트를 구현함
    - 저장 시 스키마가 없으므로 테이블(컬렉션)을 따로 만들지 않아도 됨
    - 게시글의 필드로 댓글을 추가.
    - 도큐먼트 하나의 최대 크기는 16MB이므로 한글 약 420만 자에 해당하여 서비스가 작으면 문제 없

```html
<section>
  {{#each comments}}
  <div>
    <div>
      작성자 : <b>{{name}}</b>
    </div>
    <div>
      작성일시 : {{dateString createdDt}}
      <button onclick="deleteComment('{{idx}}')">삭제</button>
    </div>
  </div>
  <div>
    <pre>{{comment}}</pre>
  </div>
  {{/each}}
</section>
```
`view`

```js
// 댓글 추가
app.post("/write-comment", async (req, res) => {
  const { id, name, password, comment } = req.body; // body에서 데이터를 가지고 오기
  const post = await postService.getPostById(collection, id); // id로 게시글의 정보를 가져오기
  console.log(post);
  // 게시글에 기존 댓글 리스트가 있으면 추가
  if (post.comments) {
    post.comments.push({
      idx: post.comments.length + 1,
      name,
      password,
      comment,
      createdDt: new Date().toISOString(),
    });
  } else {
    // 게시글에 댓글 정보가 없으면 리스트에 댓글 정보 추가
    post.comments = [
      {
        idx: 1,
        name,
        password,
        comment,
        createdDt: new Date().toISOString(),
      },
    ];
  }

  // 업데이트하기. 업데이트 후에는 상세페이지로 다시 리다이렉트
  postService.updatePost(collection, id, post);
  return res.redirect(`/detail/${id}`);
});

```
`controller`

## 7.5.10 댓글 삭제 API

- 게시글의 댓글을 삭제하는 기능을 제공하는 API를 작성함
- 삭제할 댓글 ID를 받아 삭제를 수행하고 결과를 반환하는 엔드포인트를 구현함

```js
  async function deleteComment(idx) {
    const password = prompt("삭제하려면 패스워드를 입력해주세요");
    // 프롬프트에서 취소를 누른 경우 처리 
    if (!password) {
      return;
    }

    // /delete-comment API 실행 
    const result = await fetch("/delete-comment", {
      ...deleteOption,
      body: JSON.stringify({ id: "{{post._id}}", idx, password })
    });

    // 댓글 삭제 실패 시 메시지 띄우고 함수 실행 종료
    const data = await result.json();
    if (!data.isSuccess) {
      alert("삭제에 실패했습니다. 패스워드를 확인해주세요.");
      return;
    }

    // 성공 시 메시지를 띄우고 화면 리프레시 
    alert("삭제 성공!");
    document.location.reload();
  }
```
`view`

```js
// 댓글 삭제
app.delete("/delete-comment", async (req, res) => {
  const { id, idx, password } = req.body;

  // 게시글(post)의 comments 안에 있는 특정 댓글 데이터를 찾기
  const post = await collection.findOne(
    {
      _id: ObjectId(id),
      comments: { $elemMatch: { idx: parseInt(idx), password } },
    },
    postService.projectionOption
  );

  // 데이터가 없으면 isSuccess : false를 주면서 종료
  if (!post) {
    return res.json({ isSuccess: false });
  }

  // 댓글 번호가 idx 이외인 것만 comments에 다시 할당 후 저장
  post.comments = post.comments.filter((comment) => comment.idx != idx);
  postService.updatePost(collection, id, post);
  return res.json({ isSuccess: true });
});
```
`controller`

```json
{
  "_id": {
    "$oid": "667854796395714fd5550e25"
  },
  "title": "real",
  "writer": "real",
  "password": "real",
  "content": "real",
  "hits": 4,
  "createdDt": "2024-06-23T16:59:37.287Z",
  "comments": [
    {
      "idx": 1,
      "name": "r",
      "comment": "r",
      "createdDt": "2024-06-23T16:59:41.682Z"
    },
    {
      "idx": 2,
      "name": "r2",
      "comment": "r2",
      "createdDt": "2024-06-23T16:59:45.188Z"
    },
    {
      "idx": 3,
      "name": "r3",
      "password": "r3",
      "comment": "r3",
      "createdDt": "2024-06-23T16:59:47.349Z"
    }
  ]
}
```

이슈 발견

- 현상: 맨 마지막 댓글만 지울 수 있움
- 원인: `/write-comment` 시에 다른 comment의 `password`가 지워졌음.
    - `/write-comment`는 `comment`를 포함한 `post` 정보를 모두 가져와서 새 `comment`를 push하고 `post`를 다시 쓰는 방식
    - 그런데 `projectionOption` 으로 `password` 제외가 걸려있어서 비밀번호를 빼고 가져옴. 새로 쓴 post에는 비밀번호가 사라짐.
- 조치: `projectionOption` 을 빼고 비밀번호를 서버로 냅다 가져와서 작업함.
    - 이래도 되는걸까 보안이라든지
- 생각해볼점
    - `comments`를 통채로 다시 쓰지 말고 DB 상의 array에 push할 순 없는지...

```js
async function getPostByIdWithPassword(collection, id) {
  return await collection.findOne({ _id: ObjectId(id) });
}
```

![](attachment/1e72583ff68b619f211c5d419a244a8e.png)

업그레이드 아이디어

## 추가 아이디어

- 쿼리 스트링으로 들어온 문자열을 그대로 정규식으로 사용하는데 보안 문제 있어 보임
    - const query = { title: new RegExp(search, "i") };