---
추가 일시: Invalid date
강의: 공부하다가 궁금했던것
---
- `arguments` 객체

```JavaScript
function printArgument(a,b,c){
	for(const arg of arguments){
		console.log(arg);
	}
}

printArgument('나만', '없어', '고양이');
printArgument('만두', '반으로', '잘라먹네', '부지런하다');
printArgument('만두');
```

# Rest Parameter

```JavaScript
function printArgument(...args){ => rest parmaeter
	for(const arg of args){
		console.log(arg);
	}
}

printArgument('나만', '없어', '고양이');
printArgument('만두', '반으로', '잘라먹네', '부지런하다');
printArgument('만두');
```

  

→ 두 가지 방법 의 차이는 배열 메서드를 사용할 수 있다 없다 밖에 없나?

→ 어떤걸 사용하는게 권장될까?