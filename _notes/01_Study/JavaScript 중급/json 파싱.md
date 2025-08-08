---
추가 일시: Invalid date
강의: 공부하다가 궁금했던것
---
`json()` 메소드로 **파싱(parsing)한다**는 건, 서버에서 받은 **JSON 형태의 데이터를 JavaScript 객체로 변환한다**는 뜻이야.  
  

### **JSON이란?**

JSON(**J**ava**S**cript **O**bject **N**otation)은 데이터를 전달할 때 자주 쓰이는 **텍스트 기반 데이터 형식**이야.

예를 들어, 서버에서 이런 JSON 데이터를 보낸다고 가정해보자:

```JSON
json
{
  "name": "Alice",
  "age": 25,
  "job": "developer"
}

```

이걸 **JavaScript에서 객체처럼 쓰려면** 문자열을 객체로 변환해야 하는데,

그걸 **"파싱(parsing)"**한다고 표현해.

  

### ❓ fetch만 사용하면 안되는 이유?

⇒ fetch의 응답(`response`)은 기본적으로 **ReadableStream 형태**라서, 바로 데이터에 접근할 수 없어.