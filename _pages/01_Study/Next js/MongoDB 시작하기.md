---
추가 일시: Invalid date
강의: Next.js API 만들기
---
- [[#MongoDB 소개]]
- [[#MongoDB Atlas와 Mongoose 준비하기]]
- [[#Next.js에서 환경 변수 사용하기]]
    - [[#Next.js에서 환경 변수 추가하기]]
    - [[#Next.js에서 사용하는 특별한 환경 변수]]
- [[#데이터베이스 연동하기]]
- [[#모델 만들기]]
- [[#도큐먼트 생성, 조회하기]]
    - [[#생성 - create()]]
    - [[#조회- findById() , find()]]
- [[#도큐먼트 수정, 삭제하기]]
    - [[#수정 - findByIdAndUpdate()]]
    - [[#삭제 - findByIdAndDelete()]]
    - [[#조건으로 하나만 조회]]
    - [[#조건으로 업데이트하기]]
    - [[#조건으로 삭제하기]]

---

# MongoDB 소개

  

- **데이터**
    
    : 컴퓨터가 이해할 수 있는 형태로 저장한 정보
    
- **데이터베이스**
    
    : 데이터를 따로 저장하고 관리하는 프로그램
    
    : 백엔드 서버와는 별도로 데이터베이스를 만들어 놓으면 데이터를 관리하기도 좋고 서버랑 데이터베이스 개수를 늘리기도 좋다.
    
      
    
- **MongoDB**
    
    : NoSQL
    
    - `Document` : MongoDB에 저장하는 데이터
    - `Collection` : 도큐먼트를 모아 놓은 것 ==(상품데이터는 상품데이터 끼리)==

  

---

# MongoDB Atlas와 Mongoose 준비하기

  

- **MongoDB Atlas**
    
    : 몽고DB를 만드는 회사에서 운영하는 클라우드 서비스
    

  

- **Mongoose**
    
    : 라이브러리
    
    ```TypeScript
    npm install mongoose
    ```
    

  

---

# Next.js에서 환경 변수 사용하기

  

- 환경 변수
    
    : 프로그램에서 실행 환경에 따라 다른 값을 지정할 수 있는 변수
    
      
    

### Next.js에서 환경 변수 추가하기

  

- Next.js에서는 기본적으로 `dotenv`라는 라이브러리를 지원한다.
- `.env` 파일에서 환경변수를 저장해두면 Node.js 프로젝트를 실행할 때 환경 변수로 지정해주는 라이브러리
- `.env` 파일은 소스코드에 포함시키면 안된다.

  

### Next.js에서 사용하는 특별한 환경 변수

  

- `NEXT_PUBLIC_` : 클라이언트 사이드에서 사용하는 환경 변수에 사용하는 접두사

  

---

# 데이터베이스 연동하기

  

- Mongoose에서는 MongoDB에 Connection을 만들고 Mongoose라이브러리를 통해서 여러가지 작업들을 하는 식으로 MongoDB를 사용한다.

```JavaScript
import mongoose from 'mongoose';
// ...
await dbConnect();
console.log(mongoose.connection.readyState); // 잘 접속되었는지 확인
```

  

---

# 모델 만들기

  

- **모델** : 데이터를 담을 붕어빵 틀
- **스키마** : 모델의 구조와 프로퍼티를 정하는 것
    
    ```JavaScript
    const shortLinkSchema = new mongoose.Schema({
      title: { type: String, default: "" },
      url: { type: String, default: "" },
      shortUrl: { type: String, default: "" },
    });
    ```
    

  

- 스키마를 가지고 `mongoose.model` 메소드를 호출해서 모델을 만든다
    
    - 첫번째 아규먼트는 모델의 이름 ⇒ `mongoose.models`프로퍼티에서 활용
    
    ```JavaScript
    const ShortLink =
      mongoose.models["ShortLink"] 
      || mongoose.model("ShortLink", shortLinkSchema);
    ```
    

  

- updatedAt, createdAt은 `timeStamps`로 편하게 추가할 수 있다.

```JavaScript
const shortLinkSchema = new mongoose.Schema({
  title: { type: String, default: "" },
  url: { type: String, default: "" },
  shortUrl: { type: String, default: "" },
},{
    timestamps:true
});
```

  

---

# 도큐먼트 생성, 조회하기

  

### 생성 - `create()`

```JavaScript
const newShortLink = await ShortLink.create(req.body);
res.status(201).send(newShortLink);
```

  

### 조회- `findById()` , `find()`

```JavaScript
// 특정한 id의 데이터를 가져올 때
const { id } = req.query;

case "GET":
      const shortLink = await ShortLink.findById(id);
      res.send(shortLink);
      break;
      

// 데이터 여러 개를 가져올 때
 case "GET":
      const shortLinks = await ShortLink.find();
      res.send(shortLinks);
      break;
```

  

---

# 도큐먼트 수정, 삭제하기

  

### 수정 - `findByIdAndUpdate()`

```JavaScript
 case "PATCH":
      const UpdateShortLink 
	      = await ShortLink.findByIdAndUpdate(id, req.body, {
        new: true,
      });
      res.send(UpdateShortLink);
      break;
```

⇒ **첫번째 아규먼트로 id값**을 넘겨주고

**두번째 아규먼트에는 업데이트할 데이터**를 객체로 전달

**세번째 아규먼트**에는 새로운 객체를 리턴할지 여부

  

### 삭제 - `findByIdAndDelete()`

```JavaScript
case "DELETE":
      await ShortLink.findByIdAndDelete(id);
      res.send();
      break;
```

⇒ 보통 데이터를 삭제하고 나면 상태 코드로 `204 No Content` 를 보내준다.

```JavaScript
case "DELETE":
      await ShortLink.findByIdAndDelete(id);
      res.status(204).send();
      break;
```

  

---

  

## **조건으로 하나만 조회**

아규먼트로 조건을 넘겨주고 해당하는 도큐먼트를 하나만 조회합니다.

```JavaScript

const shortLink = await ShortLink.findOne({ shortUrl: 'c0d317' });
```

## **조건으로 업데이트하기**

첫 번째 아규먼트로 넘겨준 조건에 해당하는 도큐먼트를 업데이트합니다. 두 번째 아규먼트로는 업데이트할 값을 넘겨줍니다.

```JavaScript

await ShortLink.updateOne({ _id: 'n3x7j5' }, { ... }); // 업데이트만 하고 업데이트 된 값을 리턴하지는 않음

const updatedShortLink = await ShortLink.findOneAndUpdate({ _id: 'n3x7j5' }, { ... });

```

## **조건으로 삭제하기**

아규먼트로 넘겨준 조건에 해당하는 도큐먼트를 삭제합니다.

```JavaScript

await ShortLink.deleteOne({ _id: 'n3x7j5' }, { ... }); // 삭제만 하고  기존 값을 리턴하지는 않음

const deletedShortLink = await ShortLink.findOneAndDelete({ _id: 'n3x7j5' }, { ... });
```