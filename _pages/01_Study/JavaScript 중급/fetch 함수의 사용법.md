---
추가 일시: Invalid date
강의: 공부하다가 궁금했던것
---
`fetch` 함수는 **JavaScript에서 제공하는 비동기 HTTP 요청 함수**로, 서버에서 데이터를 가져오거나(`GET`), 데이터를 보낼(`POST`, `PUT`, `DELETE` 등) 때 사용해.  

  

fetch의 특징

- 비동기 함수 → 네트워크 요청이 완료될 때 까지 코드가 멈추지 않고 계속 실행됨
- promise 반환 → then/catch 또는 async/await으로 처리
- 기본적으로 오류가 발생해도 reject되지 않음
    - http 응답 코드가 404나 500이어도 fetch는 성공한 것으로 간주
    - `.ok` 속성을 체크해서 응답 상태를 확인해야 함

  

❓ 오류가 발생해도 reject되지 않는다고 했는데 내가 알기로는 promise 상태중에 rejected라는게 있다고 들었는데 오류가 발생해도 fulfilled상태로 반환되는건가?

  

❗ `fetch` 함수는 **네트워크 오류가 발생하지 않는 한** `Promise`가 `fulfilled` 상태로 반환됨