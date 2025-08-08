---
추가 일시: Invalid date
강의: TypeScript 기본기
---
- [[#TypeScript 프로젝트 만들기]]
- [[#TypeScript가 실행되는 과정]]
- [[#타입을 정하는 법]]
- [[#배열과 튜플]]
- [[#객체 타입]]
- [[#any]]
- [[#함수에 타입 정의하기]]

---

# TypeScript 프로젝트 만들기

  

- Node.js 프로젝트에서 `package.json` 파일을 생성하는 명령어

```JavaScript
npm init
```

  

- 타입스크립트 설치

```JavaScript
npm install --save-dev typescript
```

  

- 타입스크립트를 사용할 때 필요한 설정 파일 만들기

```JavaScript
npx tsc --init
```

- `npx` : Node 모듈을 실행하는 명령어
- `tsc` : 타입스크립트 컴파일러 모듈 ( 타입스크립트 코드를 자바스크립트 코드로 바꿔주는 모듈)

  

- package.json에서 스크립트 추가

```JavaScript
"scripts": {
    "build": "tsc"
  },
```

- 타입스크립트 코드를 자바스크립트 코드로 바꿔준다

  

- 코드 실행

```JavaScript
npm run build
```

- main.ts를 실행하면 main.js가 생긴다

```JavaScript
"use strict";
console.log("typeScript");
```

- `use strict` : 자바스크립트 코드를 strict 모드로 실행할 때 사용. 좀 더 엄격한 규칙으로 자바스크립트를 실행하고 싶을 때 사용

  

# TypeScript가 실행되는 과정

  

- 타입스크립트는 자바스크립트에 문법을 추가해서 사용하는 언어(`슈퍼셋`)이다
- 그렇기 때문에 웹 브라우저나 Node.js는 타입스크립트 코드를 그대로 실행할 수 없고 자바스크립트로 변환해서 실행해야 한다
- `TSC, 타입스크립트 컴파일러` : 타입스크립트 코드를 자바스크립트 코드로 트랜스파일 해 주는 프로그램
    - `컴파일러(트랜스파일)` : 컴파일(한 프로그래밍 언어에서 다른 프로그래밍 언어로 번역하는 것)을 하는 프로그램

  

# 타입을 정하는 법

  

- 타입스크립트는 타입을 추측할 수 있는 경우 타입을 추론한다.

  

- 직접 타입을 정하는 법
    
    - 변수 이름 뒤에 `:타입이름`을 붙여준다
    
    ```JavaScript
    let size : number = 100;
    ```
    

  

- 타입은 타입 검사하는데만 영향을 끼치고 실제 코드 실행에는 영향을 주지 않는다.

  

# 배열과 튜플

- 배열

```JavaScript
const cart: string[] =[];
```

  

- 고차 배열

```JavaScript
const carts : string[][] = [
	['c001,c002'],
	['c003']
]
```

  

- `튜플` : 배열이지만 개수를 명확하게 정하고 싶을 때 사용

```TypeScript
let mySize : [number,number] = [167,28];
```

  

# 객체 타입

```TypeScript
let product: {
	id:string;
	name:string;
}
```

  

- 옵셔널 프로퍼티

```TypeScript
let product: {
	id:string;
	name:string;
	memberOnly?: boolean;
}
```

  

- 프로퍼티의 개수를 알 수 없거나 개수를 정해 놓고 싶지 않은 경우 프로퍼티 값에 타입만 지정할 수 있다.

```TypeScript
let stock: {
	[id:string] : number;
} = {
	c001:3.
	c002 : 0,
}
```

  

# any

```TypeScript
const product : any = {
	id:'c001',
	name:'코드잇',
	price: 12900,
	sizes : ['M','L','XL'],
}

console.log(product.reviews[2]);
```

- `any` : 타입 오류에서 자유롭게 만들 수 있는 타입 → 쓰는걸 권장하지 않는다 (타입스크립트의 장점이 없어지기 때문에)
- 타입을 알 수 없는 경우엔 자동으로 any 타입이 되는데 **변수에 직접 타입을 설정**하거나, `**as**`**로 타입을 명시**해주는것이 좋다

```TypeScript
const parsedProduct = JSON.parse(
	'{"name""codeit","price":12000}'
) as {
	name:string;
	price:number;
}
```

  

# 함수에 타입 정의하기

```TypeScript
function addToCart(id:string, quantity:number):boolean {
  if (stock[id] < quantity) {
    return false;
  }
```

- **파라미터의 타입**은 파라미터 뒤에 적어주고 **함수의 리턴 타입**은 괄호 다음에 : 타입

  

- **기본값** 넣어주기

```TypeScript
function addToCart(id:string, quantity:number = 1 ) {
  if (stock[id] < quantity) {
    return false;
  }
```

  

- **함수 자체에 타입** 정해주기

```TypeScript
const codeitmall : {
	stock : {[id:string]: number;
	cart: string[];
	addToCart: (id:string, quantity?:number) => boolean;
} = {
	stock: {
		c001:3,
	},
	cart:[],
	addToCart,
};
```

  

- **레스트 파라미터**(Rest Parameter) 문법을 쓰는 경우에 타입을 정의하는 법

```TypeScript
function addManyToCart(...ids : string[]) {
	for(const id of ids) {
		addToCart(id);
	}
};

const codeitmall : {
	stock : {[id:string]: number;
	cart: string[];
	addToCart: (id:string, quantity?:number) => boolean;
	addManyToCart : (...ids:string[]) => void;
} = {
	stock: {
		c001:3,
	},
	cart:[],
	addToCart,
	addManyToCart,
};
```

- 아무것도 리턴하지 않는 함수에서는 `void`라고 적어준다