---
추가 일시: Invalid date
강의: TypeScript 기본기
---
- [[#Enum]]

---

  

# Enum

- 값의 종류를 나열할 수 있는 경우에 쓸 수 있는 타입

```TypeScript
enum Size {
	S = 'S',
	M ='M',
	L = 'L',
	XL = 'XL',
}

let product : {
	id:string;
	name:string;
	price:number;
	membersOnly?:boolean;
	sizes: Size[];
} = {
	id:'c001',
	name: 'codeit',
	price:2000,
	sizes:[Size.M, Size.L],
};
```

- Enum의 기본값은 0부터 시작한다

```TypeScript
console.log(Size.S); // 0
console.log(Size.M); // 1
console.log(Size.L); // 2
```

- Enum을 사용할 때는 되도록이면 **값을 정해 놓고 쓰는 것이 좋다**.