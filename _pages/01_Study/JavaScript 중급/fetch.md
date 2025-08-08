---
추가 일시: Invalid date
강의: 자바스크립트로 리퀘스트 보내기
---
- [[#fetch 기본문법]]
- [[#다양한 GET 리퀘스트 보내기]]
- [[#POST 리퀘스트 보내기]]
- [[#API 함수 만들기]]
- [[#오류 처리하기]]
- [[#웹 브라우저에서 리퀘스트 확인하기]]

  

# fetch 기본문법

- fetch는 기본적으로 get리퀘스트를 보낸다
- 아규먼트로 URL을 전달한다
- 리스폰스가 도착하면 promise가 fulfilled상태가 되고 리스폰스를 결과값으로 받게된다
- 리스폰스를 가져오기 위해서 await문을 쓴다

  

- `status`와 `headers` 프로퍼티로 리스폰스의 상태와 헤더에 접근할 수 있다

```JavaScript
const res = await fetch();

console.log(res.status);
console.log(res.headers);
```

  

- `json 메소드`는 바디의 json 문자열을 읽어서 **자바스크립트 객체로 변환**해 줌

  

# 다양한 GET 리퀘스트 보내기

  

- `페이지네이션` : 모든 데이터를 한꺼번에 보여주지 않고 페이지처럼 나눠서 보여주는 것
- `offset` : 데이터 몇 개를 건너뛰고 요청할 것인지
- `limit` : 데이터 몇 개를 요청할 것인지
- `searchParams` 이용

```JavaScript
const url = new URL('https://.....');
url.searchParams.append('offset',10);
url.searchParams.append('limit',10);

const res = await fetch(url);
```

# POST 리퀘스트 보내기

  

- fetch 함수의 두 번째 아규먼트로 다양한 옵션을 넘겨줄 수 있다.
- post 리퀘스트를 보내려면 옵션의 method 프로퍼티를 POST로 설정하면 됨
    - method : `‘GET’,’POST’,’PATCH’,’DELETE’`같은 값을 설정 할 수 있다(기본 값 ‘GET’)
- 보통 body를 같이 전송한다.
    - 자바스크립트 객체를 json 문자열로 변환하기 위해서 `JSON.stringify`를 사용한다.
- 데이터를 보낼 때 `Content-Type 헤더`로 어떤 형식의 데이터를 보낼지 알려주는 것이 좋다

```JavaScript
fetch(`https://...`,{
	method : 'POST',
	body : JSON.stringify(surveyData),
	headers : {
		'Content-Type' : 'application/json',
		}
});
```

# API 함수 만들기

```JavaScript
// api.js

export async function getColorSurveys(params={}) {
		const url = new URL('');
		Object.keys(params).forEach((key)=>
			url.searchParams.append(key,params[key])
		);
		
		const res = await fetch(url);
		const data = await res.json();
		return data;
	}
```

```JavaScript
// main.js

import {getColorSurveys} from './api.js';

const data = await getColorSurveys({offset=10, limit=10});
console.log(data);
```

  

# 오류 처리하기

```JavaScript
export async function getColorSurveys(params={}) {
		const url = new URL('');
		Object.keys(params).forEach((key)=>
			url.searchParams.append(key,params[key])
		);
		
		const res = await fetch(url);
		
		if(!res.ok) {
			throw new Error('데이터를 불러오는데 실패했습니다.');
		}
		
		const data = await res.json();
		return data;
	}
```

- `res.ok` 는 리스폰스의 상태 코드가 2로 시작하면 true, 그렇지 않으면 false를 리턴

  

- 오류 문자열을 그대로 바디에 전달하는 경우

```JavaScript
if(!res.ok) {
	const message = await res.text(); .//(.text()도 Promise를 반환)
	throw new Error(message);
}
```

- 오류를 JSON 형식으로 돌려준다면

```JavaScript
// 리스폰스 바디
// {
//   "message": "Field 'hairType' is required"
// }

if(!res.ok){
	const body = await res.json();
	const {message} = body;
	throw new Error(message);
}
```

# 웹 브라우저에서 리퀘스트 확인하기

  

- 웹브라우저에서는 개발자도구로 네트워크 액티비티를 확인할 수 있다.