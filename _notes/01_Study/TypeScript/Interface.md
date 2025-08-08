---
추가 일시: Invalid date
강의: TypeScript 기본기
---
- [[#🌟 Interface]]

---

  

# 🌟 Interface

```TypeScript
interface Product {
	id:string;
	name:string;
	price:number;
	membersOnly?:boolean;
}

const product1:Product = {
	id:'c001',
	name:'codeit',
	price:2000,
	membersOnly:true,
}
```

- Interface는 **상속**이 가능하다

```TypeScript
interface ClothingProduct extends Product {
	sizes:Size[];
}

const product1:ClothingProduct = {
	id:'c001',
	name:'codeit',
	price:2000,
	membersOnly:true,
	sizes:[Size.M, Size,L],
}
```

- Interface로 **함수의 타입**도 정의 가능

```TypeScript
/*
const printProduct(product:Product) {
	console.log(`${product.name}의 가격은 ${product.price}원입니다.`);
}
*/

interface PrintProductFunction {
	(product: Product) : void;
}

const printProduct:PrintProductFunction = (product) => {
	console.log(`${product.name}의 가격은 ${product.price}원입니다.`);
}
```