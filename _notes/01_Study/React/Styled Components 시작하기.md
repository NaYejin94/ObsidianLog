---
추가 일시: Invalid date
강의: Styled Components
---
- [[#Styled Components 란?]]
- [[#기존 방식의 문제점]]
- [[#Nesting 문법]]
- [[#다이나믹 스타일링]]
- [[#상속]]
- [[#스타일 재사용 : css함수]]

---

# Styled Components 란?

- 컴포넌트를 만들면서 바로 해당 컴포넌트의 스타일을 작성한다.
- jsx로 컴포넌트를 만드는 것처럼 css를 쓰는 방식

  

# 기존 방식의 문제점

- CSS 클래스 이름이 겹치는 문제

```JavaScript
const StyledApp = styled.div`backgroud-color:#000000;`;
const Dashboard = styled.div`font-size:16px`;
```

- 재사용하는 CSS 코드를 관리하기 어렵다

```JavaScript
const shadow20 = css`
	box-shadow : 0 10px 15px rgba(0,0,0,0.2);
`;

const = shadow40 = css`
	box-shadow : 0 10px 15px rgba(0,0,0,0.4);
`;
```

  

# Nesting 문법

  

- `& 선택자` : 부모 선택자

```JavaScript
const Button = styled.button`
	...
	
	&:hover,
	&:active {
		...
	}
	`
```

- `${}` : 자손 결합자

```JavaScript
const StyledButton = styled.button`
	...
	& ${icon} {
		margin-right:4px;
	}
	`
```

  

# 다이나믹 스타일링

- `${…} 안에 값` 사용하기

```JavaScript
const SIZES = {
	large:24,
	medium:20,
	small:16
};

const Button = styled.button`
	...
	font-size: ${SIZE['medium']}px;
`;
```

  

- `${…} 안에 함수` 사용하기

```JavaScript
const Button = styled.button`
	...
	font-size: ${(props)=>SIZES[props.size]}px;
`
```

→ 널 병합 연산자를 사용할 수도 있다

```JavaScript
font-size: ${({size}) => SIZES[size] ?? SIZES['medium']}px;
```

  

- `논리 연산자` 사용하기

```JavaScript
const Button = styled.button`
	...
	${({round}) => round && `
		border-radius : 9999px;
		`}
	`;
```

→ round가 참이면 뒤의 값까지 계산

  

- `삼항 연산자` 사용하기

```JavaScript
border-raidus: ${({round}) => round ? '9999px' : `3px`};
```

  

# 상속

- styled() 함수

```JavaScript
const Button = styled.button`
	...
`;

const SubmitButton = styled(Button)`
	...
`;
```

→ SubmitButton은 Button을 상속받게 됨.

  

- JSX로 직접 만든 컴포넌트에 styled() 사용하기
    - 클래스 이름을 내려준 후에 styled()함수로 상속한다

```JavaScript
function TermsOfService({className}) {
	return (
		<div className={className}>
			..
		</div>
}

const StyledTermsOfService = styled(TermsOfservice)`
	...
`;		
```

  

# 스타일 재사용 : css함수

- 반복되는 코드를 한곳에서 지정하고 여러군데서 활용

```JavaScript
const Button = styled.button`
	...
	font-size: ${({size}) -> SIZES[size] ?? SIZES['medium']}px;
`

const Input = styled.input`
	...
	font-size: ${({size}) => SIZES[size] ?? SIZES['medium']}px;
`
```

→ 반복되는 구간이 있다…

```JavaScript
const fontSize = css`
	font-size: ${({size}) => SIZE[size] ?? SIZES['medium']}px;
`;

const Button = styled.button`
	...
	${fontSize}
`;

const Input = styled.input`
	...
	${fontSize}
`;
```