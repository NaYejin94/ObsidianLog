---
추가 일시: Invalid date
강의: React 웹 개발 시작하기
---
- [[#인덱스 파일에서 하는 일]]
- [[#JSX]]
- [[#프래그먼트]]
- [[#JSX에서 자바스크립트 사용하기]]
- [[#컴포넌트]]
- [[#Props]]
- [[#Children]]

---

# 인덱스 파일에서 하는 일

  

- `index.html` : 웹브라우저에서 가장 먼저 실행되는 파일
- `render` : render 메소드로 html 태그를 만들어준다

# JSX

- 자바스크립트와 HTML을 섞어서 쓸 수 있는 자바스크립트의 확장된 문법
- html태그의 class속성 → `clasName`으로 작성
- html태그의 for속성 → `htmlFor`로 작성
- 여러 단어가 조합된 속성 명 → `camel Case`로 작성
- html 태그를 작성 할 때 반드시 **하나로 감싸진 태그를 작성** 해야 함

# 프래그먼트

```JavaScript
<Fragment>
	<p>1</p>
	<p>2</p>
</Fragment>
-> 축약해서 <> 빈태그로 사용할 수도 있음
```

# JSX에서 자바스크립트 사용하기

- `{ }` : 자바 스크립트 문법은 다 사용 가능

```JavaScript
const product = '상품';

createRoot(document.getElementById('root')).render(
  <h1>{product}</h1>
)
```

```JavaScript
function handleClick() {
	alert('곧 도착합니다');
}

createRoot(document.getElementById('root')).render(
  <button onClick={handleClick}>확인</button>
)
```

  

- 중괄호 안에는 자바스크립트의 표현식만 사용할 수 있기 때문에
    
    if문이나 for문 혹은 함수 선언과 같은 **자바스크립트의 문장은 사용할 수 없다**
    

  

# 컴포넌트

- `리액트 앨리먼트` : UI를 구성하는 가장 작은 단위, 변경할 수 없는 객체
- `리액트 컴포넌트` : 여러 개의 엘리먼트를 조합하여 재사용 가능한 UI 단위를 만드는 것
    
    **엘리먼트를 반환하는 함수나 클래스**
    
      
    

```JavaScript
function Hello() {
	return <h1>안녕 리액트</h1>
}

const element = (
	<>
		<Hello />
	</>
);

createRoot(document.getElemetById('root')).render(
	element
);

=> 안녕 리액트
```

- 함수 이름의 첫 글자를 대문자로 써야 함
- JSX 문법으로 만든 리액트 엘리먼트를 리턴해 줘야 함

  

# Props

- 컴포넌트에 지정한 속성
- 컴포넌트 함수의 첫 번째 파라미터로 전달된다

# Children

- 컴포넌트의 자식들을 값으로 갖는 prop

```JavaScript
function Button({children}) {
    return <button>{children}</button>
}


<div><Button>던지기</Button></div>
```

- JSX 문법으로 컴포넌트를 작성할 때 컴포넌트를 단일 태그가 아니라 여는 태그와 닫는 태그의 형태로 작성하면, 그 안에 작성된 코드가 바로 이 `children` 값에 담기게 됩니다.