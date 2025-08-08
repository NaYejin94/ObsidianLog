---
추가 일시: Invalid date
강의: Styled Components
---
- [[#글로벌 스타일]]
- [[#애니메이션]]

---

# 글로벌 스타일

- 모든 컴포넌트에 적용하고 싶은 코드
    - 글로벌 스타일 컴포넌트를 최상위 컴포넌트에서 렌더링 하면 글로벌 스타일이 항상 적용된 상태가 되도록 할 수 있다.

```JavaScript
const GlobalStyle = createGlobalStyle`
	*{
		box-sizing: border-box;
	}
	
	body {
		font-family: 'Noto Sans KR', sans-serif;
	}
`;

function App() {
	return (
		<>
			<GlobalStyle />
			<div>글로벌 스타일</div>
		</>
	);
}

export default App;
```

  

# 애니메이션

- keyframes 함수 : styled 함수와 마찬가지로 템플릿 리터럴로 사용하는 태그함수