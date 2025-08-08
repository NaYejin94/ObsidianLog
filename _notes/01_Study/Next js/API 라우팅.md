---
추가 일시: Invalid date
강의: Next.js API 만들기
---
- [[#API 라우트 만들기]]
- [[#리퀘스트 다루기]]
- [[#리스폰스 다루기]]

---

# API 라우트 만들기

```TypeScript
export default function handler(request, response) {
  response.send("안녕 API!");
}
```

  

---

# 리퀘스트 다루기

- `method` : 리퀘스트로 들어온 HTTP 메소드 값
- `query` : 쿼리 스트링이나 Next.js에서 사용하는 Params 값이 들어 있는 객체
- `body` : 리퀘스트의 바디 값
- `cookies` : 리퀘스트의 쿠키가 키/밸류로 들어있는 객체

  

  

---

# 리스폰스 다루기

- `status()` : 리스폰스로 보낼 HTTP 상태 코드를 지정
- `send()` : 리스폰스로 보낼 바디를 전달