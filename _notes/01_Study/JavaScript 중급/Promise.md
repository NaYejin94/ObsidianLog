---
추가 일시: Invalid date
강의: 비동기 자바스크립트
---
- [[#Promise]]
- [[#await 문법]]
- [[#async 함수]]
- [[#효율적인 비동기 코드]]
- [[#async 함수의 리턴 값]]
- [[#then() 메소드]]
- [[#Promise.all() 메소드]]

  

# Promise

- 비동기 작업이 완료되면 값을 알려주는 객체
- 작업이 완료되면 값을 알려줄 것을 약속함
- 일단 promise를 돌려주고 나중에 작업이 완료되면 결과값을 promise에 채워 넣어줌

  

- promise를 다루는 방법
    - `then()` 메소드 + `콜백`
    - `async`와 `await` 문법

  

# await 문법

- `fetch` 함수 : promise 객체를 리턴
    - promise는 3개지 중 하나의 상태를 가짐
        - 비동기 작업의 결과를 기다리고 있을 때 ⇒ `Pending`
        - 비동기 작업이 성공적으로 완료됐을 때 ⇒ `Fulfilled`
        - 비동기 작업이 중간에 실패했을 때 ⇒ `Rejected`
- 비동기 작업이 완료되면 promise가 결과 값을 알려줌
- 결과값을 받아오려면 `await` 문법을 쓰면 됨
    - `await` 문법을 쓰게 되면 `promise`가 `Fulfilled` 또는 `Rejected` 상태가 될 때까지 기다림

```JavaScript
const response = await fetch(......);
const data = await response.json(); 
=> json 메소드로 리스폰스를 파싱해서 데이터 변수에 저장
console.log(data);
```

  

⇒ ==**!요약**==

Promise를 리턴하는 표현식이 있다면 **await 키워드를 써서 결과값**을 받아올 수 있다!

await 를 쓰면 **Promise가 Fulfilled 상태가 될 때까지 기다렸다가** 결과값을 리턴하기 때문

  

# async 함수

- await 는 최상위 모델이나 async 함수 안에서만 사용할 수 있다.

```JavaScript
export async function printEmployees() {
	const response = await fech('');
	const data = response.json();
	console.log(data);
}

export const printEmployeesArrow = async () => {
	const response = await fech('');
	const data = response.json();
	console.log(data);
}
```

# 효율적인 비동기 코드

```JavaScript
async function printEmployees {
	for(let i=1; i<11; i++){
		const response = await fetch(`https://...../${i}`);
		const data = response.json();
		console.log(data);
	}
}

printEmployees();
```

**효율적으로 바꿔보자 !** ⬇️

```JavaScript
async function printEmployees(id) {
	const response = await fetch(`https://...../${id}`);
	const data = response.json();
	console.log(data);
}

for(let i=1;i<11;i++){
	printEmployees(i);
}
```

⇒ for문을 풀어쓰면…

> [!important]
> 
> ```JavaScript
> async function printEmployees(id) {
> 	const response = await fetch(`https://...../${id}`);
> 	const data = response.json();
> 	console.log(data);
> }
> 
> printEmployees(1);
> printEmployees(2);
> printEmployees(3);
> printEmployees(4);
> .
> .
> .
> ```

⇒ **순서가 뒤죽박죽 출력되는 이유는…**

리퀘스트를 보낸 순서대로 리스폰스가 돌아온다는 보장이 없기 때문에…

# async 함수의 리턴 값

```JavaScript
async function getEmployees() {
	const response = await fech('');
	const data = await response.json();
	return data;
}

const employees = getEmployees();
console.log(employees);

// Promise{ <pending> }
```

==**⇒ async 함수는 항상 Promise를 리턴하기 때문!**==

```JavaScript
const employees = await getEmployees();
console.log(employees);
```

⇒ await을 써주면 결과값이 의도대로 반환된다.

⇒ 실행순서

getEmployees 함수를 실행하게 되면 pending 상태의 Promise가 바로 리턴

→ getEmployees함수가 모든 작업을 마치고 data를 리턴하게 되면

→ pending 상태가 Fulfilled상태로 바뀌면서 그 값이 employees에 할당

  

# then() 메소드

```JavaScript
async function printEmployees(id) {
	const response = await fetch(`https://...../${id}`);
	const data = response.json();
	console.log(data);
}
```

```JavaScript
const dataPromise = fetch('https://....')
										.then((response)=>response.json());
dataPromise.then((data)=>console.log(data));
```

- then 메소드는 앞선 비동기 작업이 완료되면 등록된 콜백을 실행해줌
- then메소드도 Promise리턴

```JavaScript
 fetch('https://.....')
	 .then((response)=>response.json())
	 .then((data)=>console.log(data)); =>프로미스 체인
```

⇒ 1. fetch가 반환한 promise가 fulfilled 상태가 되면 promise의 결과값을 첫번째 then의 콜백의 파라미터로 넘김

1. promise가 결과를 리턴하면 then 메소드가 리턴한 promise도 동일한 상태와 결과값을 가지게 됨
2. fulfilled상태가 되면 다음 then 메소드에 등록된 콜백이 실행
3. 두번째 콜백은 데이터를 출력하고 console.log를 리턴하는데 console.log는 undefined를 리턴하기 때문에 결국 콜백은 undefined를 리턴한다
4. 마지막 promise는 undefined를 결과값으로 가지게 된다
5. 무언가를 이어서 하지 않을 것이기 때문에 마지막 프로미스는 신경쓰지 않아도 된다……..

# Promise.all() 메소드

```JavaScript
async function printEmployees(id) {
	const response = await fetch(`https://...../${id}`);
	const data = response.json();
	console.log(data);
}

for(let i=1;i<11;i++){
	printEmployees(i);
}
```

⬇️배열에 저장했다가 한꺼번에 출력하기

```JavaScript
async function getEmployees(id) {
	const response = await fetch(`https://...../${id}`);
	const data = response.json();
	return data;
}

for(let i=1;i<11;i++){
	// const employee = await getEmployees(i);
	// => 리퀘스트를 보내고 파싱하는 작업을 순차적으로 하게 됨
}
```

- `promise.all()` : 여러 promise를 동시에 기다릴 때 사용

```JavaScript
async function getEmployees(id) {
	const response = await fetch(`https://...../${id}`);
	const data = response.json();
	return data;
}

const promises = [];

for(let i=1;i<11;i++){
	promises.push(getEmployees(i));	
}

const employees = await Promise.all(promises);
```

⇒ promise 배열은 promise를 리턴한다

⇒ 아규먼트들이 모두 fulfilled상태가 되면 배열이 반환하는 promise상태도 fulfilled가 된