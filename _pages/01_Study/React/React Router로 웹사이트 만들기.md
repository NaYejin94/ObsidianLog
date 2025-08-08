---
추가 일시: Invalid date
강의: React로 웹사이트 만들기
---
- [[#리액트로 웹사이트 만들기]]
- [[#리액트 라우터 설치하기]]
- [[#Routes로 페이지 나누기]]
- [[#Link로 이동하기]]
- [[#NavLink로 네비게이션 구현하기]]
- [[#useParams로 동적인 경로 만들기]]
- [[#없는 페이지 처리하기]]
- [[#Navigate로 리다이렉트 하기]]
- [[#useSearchParams로 쿼리 사용하기]]
- [[#useNavigate로 페이지 이동하기]]
- [[#Link, Navigate, useNavigate는 언제 쓰는게 좋을까?]]

---

# 리액트로 웹사이트 만들기

- 리액트 라우터 : 공통된 레이아웃으로 컴포넌트를 지정하고 각페이지맞다 컴포넌트를 지정할 수 있다.
- 리액트 라우터를 사용하면 여러 페이지를 나누고 이동하는 걸 컴포넌트로 할 수 있다는것이 핵심

# 리액트 라우터 설치하기

  

- 패키지 : 자바스크립트 모듈을 모아놓은 묶음
- npm : 패키지를 관리하는 프로그램

```JavaScript
npm install react-router-dom@6
```

  

# Routes로 페이지 나누기

- path 프롭으로 경로를 지정하고, elemetn프롭으로 컴포넌트를 지정
- element프롭은 jsx를 넘겨준다

```JavaScript
<BrowserRouter>
	<App>
		<Routes>
			<Route path="/" element={<Homepage />} />
		</Routes>
	<App>
</BrowserRouter>
```

  

- Routes를 렌더링할 때 리액트 라우터는 Routes안에있는 Route를 차례대로 검사하면서 현재 경로가 path와 일치하는지 하나씩 검사합니다.
- 일치하는 경로를 찾으면 element 프롭으로 지정한 컴포넌트를 렌더링해준다

  

# Link로 이동하기

```JavaScript
<Link to="/"><img src={logoImg} alt="Codethat Logo"/></Link>
<li><Link to="/courses">카탈로그</Link></li>
```

- `Link컴포넌트`에서 `to라는 프롭`으로 경로를 지정해준다
- `/` 는 절대경로로 [localhost:0000](http://localhost:0000) 에 /주소 로 이동하게 된다

  

- `slug` : 각 데이터를 구분하는 고유한 문자열, 웹개발에서는 ID보다 좀 더 의미있는 주소를 만들 때 사용

  

# NavLink로 네비게이션 구현하기

  

- `NavLink` : 메뉴에서 사용하는 링크. style 프롭으로 함수를 지정해 줄 수 있음

```JavaScript
<NavLink to="/questions" style={getLinkStyle}>커뮤니티</NavLink>
```

  

- `isActive` : Boolean형. 현재 페이지의 경로가 내비게이션의 링크에 해당하면 true반환
    
    : 리액트 인라인 스타일 객체를 리턴
    

```JavaScript
function getLinkStyle({isActive}){
	return {
		textDecoration: isActive ? 'underline' : undefined,
	}
}
```

  

# useParams로 동적인 경로 만들기

- 파라미터 : 어떤 변수로 경로를 받아온다?

```JavaScript
<Route path=":courseSlug" element={<CoursePage />} />
```

- `useParams`가 리턴하는 객체에는 현재 경로의 파라미터들이 저장되어 있다.

```JavaScript
const {courseSlug } = useParams();
const course = getCourseBySlug(courseSlug);
```

  

# 없는 페이지 처리하기

- Route 맨 마지막에 모든 경로를 포함하는 Route를 추가하면 된다.

```JavaScript
<Route path="*" element={<NotFoundPage />}/>
```

  

# Navigate로 리다이렉트 하기

- `리다이렉트` : 어떤 페이지로 접속했는데 어떤 이유때문에 다른 페이지로 이동시키는것

```JavaScript
if(!course) {
	return <Navigate to="/courses" />
}
```

  

# useSearchParams로 쿼리 사용하기

- `useSearchParams` : 쿼리 파라미터값을 가져오고 싶을 때
- get함수를 통해서 안에 있는 값을 가져올 수 있다.

```JavaScript
const [searchParams, setSearchParams] = useSearchParams();
const initKeyword = searchParams.get('keyword');
```

  

# useNavigate로 페이지 이동하기

- `useNavigate` : 코드를 사용해서 이동해야하는 경우 사용

  

# Link, Navigate, useNavigate는 언제 쓰는게 좋을까?

  

- `Link` : 사용자가 클릭해서 페이지를 이동하도록 할 때 사용
    - 하이퍼 링크 텍스트나 페이지를 이동하는버튼, 이미지에는 사용하지 않는것을 권장

  

- `Navigate` : 특정 경로에서 렌더링 시점에 다른 페이지로 이동시키고 싶을 때 사용
    - 쇼핑몰의 회원 전용 페이지에 로그인 없이 들어와서 로그인 페이지로 리다이렉트하는 경우
    - 쇼핑몰의 상품 상세 페이지에서 제품이 품절되었거나 삭제되어서 다른 페이지로 이동시키는 경우

  

- `useNavigate` : 특정한 코드의 실행이 끝나고 나서 페이지를 이동시키고 싶을 때
    - 쇼핑몰에서 장바구니에 담기를 눌렀을 때 리퀘스트를 보내고 장바구니 페이지로 이동시키는 경우
    - 쇼핑몰에서 결제하기 버튼을 누르고 나서 모든 결제가 완료된 후에 페이지를 이동시키는 경우
    - 리다이렉트된 로그인 페이지에서 로그인을 완료한 후에 처음 진입했던 페이지로 돌아가는 경우