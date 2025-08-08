## HTML란?

### **HTML이란?**

HTML은 "**Hyper Text Markup Language**"의 약자로, 웹페이지를 만들 때 사용하는 가장 기본적인 언어입니다.

  

### **쉽게 비유하자면:**

- 집을 지을 때 **기초 골격을 만드는 것과 같아요**
- 신문이나 잡지의 구조를 잡는 것과 비슷합니다
- 🤷🏻 그런데 태그 없어도 화면에 내용 보이는데요?
    - 하지만 브라우저에서 적절하게 해석하고 화면에 그림을 그리기 위해선 필요합니다.
    - 즉, 구조 짜기 위해 알아야 합니다.

  

### **HTML로 할 수 있는 것들:**

- 텍스트 넣기
- 이미지 넣기
- 링크 만들기
- 표 만들기
- 목록 만들기

  

### **간단한 예시:**

```HTML
<html> 
	<head> 
		<title>제 첫 웹페이지</title> 
	</head> 
	<body> 
		<h1>안녕하세요!</h1> 
		<p>이것은 문단입니다.</p> 
	</body>
</html>
```

### **특징:**

- 태그(`<` `>`)를 사용해서 내용을 감싸요
- 시작 태그와 끝 태그가 있어요 (예: `<p>` 시작, `</p>` 끝)
- 웹 브라우저가 이 언어를 읽고 화면에 표시해줍니다

  

### 자주 쓰이는 태그 모음:

- `html`
    - html 문서
        - html 태그 내에 모든 태그 작성
- `head`
    - 웹사이트의 정보들의 모임
    - 화면에 보이는 부분 X
    - 제목, 설명, 소셜 네트워크에 링크 남길 때 이미지, css link 태그
- `title`
    - 웹사이트 제목
- `link`
    - 외부 파일 연결할 때 사용
    - ex) css 파일 연결, 구글 폰트 연결 등
- `body`
    - 웹사이트 내용
    - 화면에 보이는 부분
- `h1`, `h2`, `h3`, `h4`, `h5`, `h6`
    - 웹사이트 내용의 제목
    - 큰 글씨 or 제목
- `p`
    - paragraph(문단)의 약자
    - 긴 글 쓰려고 할 때 많이 사용
- `div`
    - **div**ision의 약자
    - 레이아웃 나눌 때 필요함
    - 공간의 좌우 전체를 차지 (block 태그)
- `span`
    - 작성한 글자 공간 만큼만 차지 (inline 태그)
    - 좌우로 글자를 배치할 때 둘 때 사용
    - `div` or `p` 태그 안에 특정 문자만 다르게 표시하고 싶을 때 많이 사용
- `img`
    - 이미지
    - src 속성에 주소를 넣어야 함
- `a`
    - 하이퍼링크
    - 페이지 이동 시 사용
- `input`
    - 입력창 태그
    - 로그인, 회원가입, 검색 시 사용
- `button`
    - 버튼 태그

  

## div 태그는 모든 걸 할 수 있어요. 하지만!

> [!important] **여러 가지 태그**를 사용하는 것이 좋습니다. **왜 여러 태그를 써야 할까요?**

### 여러 가지 태그를 쓰는 이유

1. **의미가 더 명확해집니다. (Semantic tag)**
    
    - `<header>`: "아, 이건 머리글이구나!"
    - `<nav>`: "여기는 메뉴(네비게이션)가 있는 곳이구나!"
    - `<main>`: "이건 주요 내용이구나!"
    - `<footer>`: "이건 바닥글이구나!"
    
    → 이렇게 의미에 맞는 태그를 쓰면 🧑🏻‍💻 **코드를 읽는 사람**이 더 쉽게 이해할 수 있어요!
    
    - `div` 태그로만 이루어진 **html**
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        </head>
        
        <body>
        	<div>메뉴</div>
        	<!-- hr 태그는 수평선을 그어주는 태그입니다. -->
        	<hr />
        	<div>머릿글</div>
        	<hr />
        	<div>본문</div>
        	<hr />
        	<div>바닥글</div>
        </body>
        
        </html>
        ```
        
    - 의미가 명확한 태그로 이루어진 **html** 
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        </head>
        
        <body>
        	<nav>메뉴</nav>
        	<!-- hr 태그는 수평선을 그어주는 태그입니다. -->
        	<hr />
        	<header>머릿글</header>
        	<hr />
        	<main>본문</main>
        	<hr />
        	<footer>바닥글</footer>
        </body>
        
        </html>
        ```
        
    
      
    
2. **검색엔진이 더 잘 이해해요**
    - 구글이나 네이버 같은 검색엔진이 내 웹사이트를 더 잘 파악할 수 있어요
    - 예를 들어, `<h1>`태그는 _**"아, 이게 중요한 제목이구나!"**_라고 인식해요
    - `<div>`만 쓰면 _**"음... 이게 뭘 의미하는 거지?"**_ 하고 헷갈려해요

  

1. **접근성이 좋아져요**
    
    - 시각장애인이 사용하는 스크린리더가 웹사이트를 더 잘 읽을 수 있어요
    - `<button>`태그는 _**"이건 버튼이에요!"**_라고 알려주지만
    - `<div>`로 만든 버튼은 그냥 지나칠 수 있어요
    
    - `div` 태그를 버튼처럼 만든 예시 코드 (아래의 css는 아래에서 학습하니 당황하지 않으셔도 됩니다 ^^)
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		/* body 태그 내부의 div 태그를 버튼처럼 css를 적용해봤어요. */
        		.div-btn {
        			padding: 10px;
        			border-radius: 5px;
        			border: 1px solid gray;
        			width: 60px;
        			text-align: center;
        			cursor: pointer;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="div-btn">div 버튼</div>
        	<button>일반 버튼</button>
        </body>
        
        </html>
        ```
        

## CSS란?

CSS는 '**Cascading Style Sheets**'의 약자로, 웹페이지를 예쁘게 꾸미는 '**디자인 도구**'라고 생각하시면 됩니다.

### 쉽게 비유하자면:

- HTML이 집의 구조(뼈대)라면
- CSS는 집의 인테리어를 담당합니다

### CSS로 할 수 있는 것들:

- 글자 크기, 색상, 폰트 변경
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		.font-apply {
    			/* 폰트 크기 */
    			font-size: 20px;
    			/* 폰트 색상 */
    			color: blue;
    			/* 폰트 굵기 */
    			font-weight: bold;
    			/* 폰트 패밀리 */
    			font-family: 'Nanum Gothic', '나눔고딕', 'Malgun Gothic', '맑은 고딕', sans-serif;
    		}
    	</style>
    </head>
    
    <body>
    	<div class="font-apply">폰트 적용</div>
    </body>
    
    </html>
    ```
    
- 여백 및 테두리 지정
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		/* 여백 및 테두리 */
    		.space-border {
    			/* 요소 밖의 여백 */
    			margin: 10px;
    			/* 요소 안의 여백 */
    			padding: 10px;
    			/* 테두리 */
    			border: 1px solid black;
    		}
    	</style>
    </head>
    
    <body>
    	<div class="space-border">여백 및 테두리</div>
    </body>
    
    </html>
    ```
    
- 배경색 설정
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		/* 배경색 설정 */
    		.background-color {
    			/* 배경색 */
    			background-color: skyblue;
    		}
    	</style>
    </head>
    
    <body>
    	<div class="background-color">배경색 설정</div>
    </body>
    
    </html>
    ```
    
- **⚠️(가장 중요)** 요소들의 배치 조정 
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		/* 레이아웃 */
    		.layout {
    			/* 요소들을 가로로 배치 */
    			display: flex;
    			/* 요소들을 가로의 중앙 정렬 */
    			justify-content: center;
    			/* 요소들을 세로의 중앙 정렬 */
    			align-items: center;
    			/* 요소들의 높이 */
    			height: 200px;
    			border: 1px solid black;
    		}
    
    		/* 레이아웃 내부의 div 태그 */
    		.layout div {
    			/* 요소들의 너비 */
    			width: 100px;
    			/* 요소들의 높이 */
    			height: 100px;
    			/* 요소들의 테두리 */
    			border: 1px solid black;
    		}
    	</style>
    </head>
    
    <body>
    	<div class="layout">
    		<div>1</div>
    		<div>2</div>
    		<div>3</div>
    	</div>
    
    </body>
    
    </html>
    ```
    

  

이렇게 CSS는 웹페이지의 각 요소들을 어떻게 보여줄지 결정하는 '**스타일 규칙**'을 정의하는 언어입니다. HTML이 웹페이지의 내용을 담당한다면, CSS는 그 내용을 어떻게 예쁘게 보여줄지를 담당한다고 이해하시면 됩니다! 😊

  

## 여백을 만드는 법 - padding & margin

### **📌 개념 설명**

`**padding**` **(패딩)**

- 콘텐츠와 테두리(border) 사이의 **안쪽 여백f**
- "**내부 여백**"이라고 생각하면 됩니다
- 배경색이 적용되는 영역입니다

`**margin**` **(마진)**

- 요소와 요소 사이의 **바깥쪽 여백**
- "**외부 여백**"이라고 생각하면 됩니다
- 배경색이 적용되지 않는 영역입니다

  

**사진 예시**

- 파란색 부분이 `padding`
- 붉은색 부분이 `margin`

![[dcc80cd9-0a70-4892-9bea-3336cb9c6f87.png]]

### padding 연습하기

1. `padding`을 사용해서 div의 내부 공간을 **상하좌우 20px씩** 띄워보세요
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		.box {
    			width: 100px;
    			height: 100px;
    			background-color: pink;
    			/* padding을 사용해서 텍스트를 상하좌우 20px씩 띄워보세요 */
    		}
    	</style>
    </head>
    
    <body>
    	<div class="box">
    		안녕하세요
    	</div>
    
    </body>
    
    </html>
    ```
    
    - 해설 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.box {
        			width: 100px;
        			height: 100px;
        			background-color: pink;
        			/* padding을 사용해서 텍스트를 상하좌우 20px씩 띄워보세요 */
        			padding: 20px;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="box">
        		안녕하세요
        	</div>
        
        </body>
        
        </html>
        ```
        

  

1. padding을 사용하여 위: 30px, 오른쪽: 20px, 아래: 30px, 왼쪽: 20px의 패딩을 적용해보세요
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		.card {
    			background-color: \#f0f0f0;
    			/* 위: 30px, 오른쪽: 20px, 아래: 30px, 왼쪽: 20px의 패딩을 적용해보세요 */
    		}
    	</style>
    </head>
    
    <body>
    	<div class="card">
    		<h2>제목</h2>
    		<p>내용이 들어갑니다.</p>
    	</div>
    
    </body>
    
    </html>
    ```
    
    - 해설 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.card {
        			background-color: \#f0f0f0;
        			/* 위: 30px, 오른쪽: 20px, 아래: 30px, 왼쪽: 20px의 패딩을 적용해보세요 */
        			padding: 30px 20px 30px 20px;
        			/* 혹은 */
        			/* 상하: 30px, 좌우: 20px */
        			/* padding: 30px 20px; */
        		}
        	</style>
        </head>
        
        <body>
        	<div class="card">
        		<h2>제목</h2>
        		<p>내용이 들어갑니다.</p>
        	</div>
        
        </body>
        
        </html>
        ```
        

  

### margin 연습하기

1. 모든 `div` 태그의 외부에 **상하좌우 20px**의 여백을 만들어보세요.
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		.box {
    			width: 100px;
    			height: 100px;
    			background-color: pink;
    			/* 모든 박스의 상하좌우에 20px의 여백을 만들어보세요 */
    		}
    	</style>
    </head>
    
    <body>
    	<div class="box">
    		첫 번째 박스
    	</div>
    	<div class="box">
    		두 번째 박스
    	</div>
    </body>
    
    </html>
    ```
    
    - 해설 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.box {
        			width: 100px;
        			height: 100px;
        			background-color: pink;
        			/* 모든 박스의 상하좌우에 20px의 여백을 만들어보세요 */
        			margin: 20px;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="box">
        		첫 번째 박스
        	</div>
        	<div class="box">
        		두 번째 박스
        	</div>
        </body>
        
        </html>
        ```
        
2. `class`가 `title`인 `div` 태그의 **위쪽에만 50px**의 여백을 만들어보세요
    
    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Document</title>
    	<style>
    		.title {
    			background-color: lightblue;
    			/* 위쪽에만 50px의 여백을 만들어보세요 */
    		}
    	</style>
    </head>
    
    <body>
    	<div class="title">
    		안녕하세요!
    	</div>
    </body>
    
    </html>
    ```
    
    - 해설 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.title {
        			background-color: lightblue;
        			/* 위쪽에만 50px의 여백을 만들어보세요 */
        			margin-top: 50px;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="title">
        		안녕하세요!
        	</div>
        </body>
        
        </html>
        ```
        
    
      
    

## 배치를 바꾸는 법 1) position: 위치 바꾸기

### **📌 CSS Position** 

CSS Position은 웹페이지에서 **요소들을 어디에 배치할지 결정하는 속성**입니다.

  

**5가지 Position 값:**

1. **static** (기본값)
    
    - _**"그냥 거기 있어!"**_
    - 일반적인 문서 흐름(순서)대로 배치
    
    - 예시 코드 
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		/* 기본값이기 때문에 작성하지 않아도 position: static으로 적용됩니다. */
        		div {
        			position: static;
        		}
        	</style>
        </head>
        
        <body>
        	<div>배치1</div>
        	<div>배치2</div>
        </body>
        
        </html>
        ```
        
2. **relative**
    
    - _**"원래 자리에서 조금만 움직여!"**_
    - 자기 원래 위치 기준으로 이동
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		div {
        			width: 100px;
        			height: 100px;
        			background-color: pink;
        			margin: 10px;
        		}
        
        		.box2 {
        			/* position: relative는 기본 위치를 기준으로 움직입니다. */
        			position: relative;
        			/* 왼쪽에서 오른족 방향으로 50px 움직이기 */
        			left: 50px;
        			/* 위에서 아래 방향으로 20px 움직이기 */
        			top: 20px;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="box1">기본 박스</div>
        	<div class="box2">움직이는 박스</div>
        </body>
        
        </html>
        ```
        
3. **absolute**
    
    - _**"내 마음대로 아무 곳이나 갈래!"**_
    - `position: static`이 **아닌 속성**을 가진 가장 가까운 부모를 기준으로 자유롭게 배치
    - **일반적**으로 `position: relative`를 부모로 둡니다.
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.container {
        			width: 300px;
        			height: 400px;
        			background-color: lightgray;
        			position: relative;
        		}
        
        		.btn {
        			background-color: yellow;
        			padding: 10px;
        			/* position: absolute는 부모 요소 중 position이 relative인 요소를 기준으로 위치합니다. */
        			position: absolute;
        			/* 오른쪽에서 왼쪽 방향으로 0px 위치하기 - 오른쪽 밀착 */
        			right: 0;
        			/* 아래에서 위 방향으로 0px 위치하기 - 아래 밀착 */
        			bottom: 0;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="container">
        		<img
        			src="https://images.unsplash.com/photo-1560781290-7dc94c0f8f4f?q=80&w=1635&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
        			alt="고기 사진" width="100%" height="100%" />
        
        		<div class="btn">
        			좋아요 버튼
        		</div>
        	</div>
        </body>
        
        </html>
        ```
        
4. **fixed**
    
    - _**"화면에 딱 붙어있을게!"**_
    - 스크롤해도 화면에서 움직이지 않음
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.header {
        			background-color: skyblue;
        			padding: 10px;
        			/* position: fixed를 이용하여 브라우저 최상단에 고정합니다. */
        			position: fixed;
        			/* 위에서 아래 방향으로 0px 위치하기 - 위 밀착 */
        			top: 0;
        			/* 왼쪽에서 오른쪽 방향으로 0px 위치하기 - 왼쪽 밀착 */
        			left: 0;
        			/* 너비를 100%로 설정하여 화면 너비와 동일하게 설정합니다. */
        			width: 100%;
        		}
        
        		.content {
        			/* 헤더에 가려져 있는 것을 방지하기 위해 위쪽 여백을 줍니다. */
        			padding-top: 40px;
        			/* 일부러 스크롤을 만들기 위해 높이를 크게 했습니다. */
        			height: 1000px;
        			background-color: lightgray;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="header">
        		상단 메뉴
        	</div>
        
        	<div class="content">긴 내용이 있다고 가정해봅시다...</div>
        </body>
        
        </html>
        ```
        
5. **sticky**
    
    - _**"스크롤하다가** ==**특정 위치**==**에서 붙어있을게!"**_
    - 스크롤 위치에 따라 `fixed`처럼 동작
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		header {
        			background-color: skyblue;
        			padding: 10px;
        		}
        
        		.sticky-nav {
        			padding: 10px;
        			background-color: red;
        			/* 스크롤 시 고정 */
        			position: sticky;
        			/* 위에서 아래 방향으로 0px 위치하기 - 위 밀착 */
        			top: 0;
        		}
        
        		.content {
        			/* 헤더에 가려져 있는 것을 방지하기 위해 위쪽 여백을 줍니다. */
        			padding-top: 40px;
        			/* 일부러 스크롤을 만들기 위해 높이를 크게 했습니다. */
        			height: 1000px;
        			background-color: lightgray;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="container">
        		<header>맨 위 내용</header>
        
        		<nav class="sticky-nav">
        			네비게이션 메뉴
        		</nav>
        
        		<div class="content">
        			<p>긴 내용 1...</p>
        			<p>긴 내용 2...</p>
        			<p>긴 내용 3...</p>
        		</div>
        	</div>
        </body>
        
        </html>
        ```
        

  

## 배치를 바꾸는 법 2) flex: 정렬하기

### 📌 **CSS Flex 기본 개념**

`Flexbox`는 요소들을 **행과 열 형태**로 쉽게 배치할 수 있게 해주는 CSS 레이아웃 모델입니다.

**기본 용어:**

- **flex container**: display: flex를 설정한 부모 요소
- **flex items**: container 안에 있는 자식 요소들
- **main axis**: 주축 (가로 또는 세로)
- **cross axis**: 교차축 (주축의 수직 방향)

  

1. **기본적인 가로 배치**
    
    - 정렬하고 싶은 부모 태그에 `display: flex`를 작성합니다.
    - 자식 태그들은 **가로로 정렬**됩니다. (원래 `div` 태그와 같은 **block 태그**는 위 아래로 배치됩니다.)
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.container {
        			/* Flex 컨테이너로 설정 */
        			display: flex;
        			background: \#f0f0f0;
        		}
        
        		.item {
        			background: skyblue;
        			margin: 5px;
        			padding: 20px;
        		}
        	</style>
        </head>
        
        <body>
        	<!-- flex 컨테이너 -->
        	<div class="container">
        		<!-- flex 아이템 -->
        		<div class="item">1</div>
        		<div class="item">2</div>
        		<div class="item">3</div>
        	</div>
        </body>
        
        </html>
        ```
        

  

1. **요소들 간격 조절 (justify-content)**
    
    - `display: flex`를 작성한 곳에 `justify-content`를 작성하고 아래 옵션들 중 하나를 선택합니다.
    - 요소들 사이에 균등한 간격: `space-between`
    - 요소들을 시작 지점으로 정렬: `flex-start`
    - 요소들을 끝 지점으로 정렬: `flex-end`
    - 요소들을 가운데로 정렬: `center`
    - 요소들 사이에 균등한 간격 정렬: `space-around`
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.container {
        			/* Flex 컨테이너로 설정 */
        			display: flex;
        			/* 요소들 사이에 균등한 간격 */
        			justify-content: space-between;
        			/* 다른 옵션: */
        			/* 요소들을 시작 지점으로 정렬 */
        			/* justify-content: flex-start; */
        			/* 요소들을 끝 지점으로 정렬 */
        			/* justify-content: flex-end; */
        			/* 요소들을 가운데로 정렬 */
        			/* justify-content: center; */
        			/* 요소들 사이에 균등한 간격 */
        			/* justify-content: space-around; */
        			background: \#f0f0f0;
        		}
        
        		.item {
        			background: skyblue;
        			margin: 5px;
        			padding: 20px;
        		}
        	</style>
        </head>
        
        <body>
        	<!-- flex 컨테이너 -->
        	<div class="container">
        		<!-- flex 아이템 -->
        		<div class="item">1</div>
        		<div class="item">2</div>
        		<div class="item">3</div>
        	</div>
        </body>
        
        </html>
        ```
        

  

1. **세로 정렬 (align-items)**
    
    - `display: flex`를 작성한 곳에 `align-items`를 작성하고 아래 옵션들 중 하나를 선택합니다.
    - 세로 방향(교차축)의 중앙으로 정렬: `center`
    - 요소들을 세로(교차축) 방향으로 늘어나게 함 (기본값): `stretch`
    - 요소들을 시작 지점으로 정렬: `flex-start`
    - 요소들을 끝 지점으로 정렬: `flex-end` (박스들의 위쪽 경계선이 일직선상에 정렬)
    - 요소들을 기준선에 맞춤: `baseline` (텍스트들이 마치 한 줄에 쓴 것처럼 정렬)
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>Document</title>
        	<style>
        		.container {
        			display: flex;
        			/* 세로 중앙 정렬 */
        			align-items: center;
        			/* 다른 옵션: */
        			/* 요소들을 늘어나게 함 (기본값) */
        			/* align-items: stretch는 flex 아이템들을 컨테이너의 교차축 방향으로 늘어나게 합니다. */
        			/* 즉, 세로 방향으로 컨테이너 높이만큼 늘어납니다. */
        			/* align-items: stretch; */
        			/* 요소들을 시작 지점으로 정렬 */
        			/* align-items: flex-start; */
        			/* 요소들을 끝 지점으로 정렬 */
        			/* align-items: flex-end; */
        			/* 요소들을 기준선에 맞춤 */
        			/* align-items: baseline; */
        
        			background: \#f0f0f0;
        			padding: 10px;
        			height: 150px;
        		}
        
        		.item {
        			background: skyblue;
        			padding: 20px;
        			margin: 5px;
        		}
        
        		.tall {
        			height: 80px;
        		}
        	</style>
        </head>
        
        <body>
        	<!-- flex 컨테이너 -->
        	<div class="container">
        		<div class="item">작은 박스</div>
        		<div class="item tall">큰 박스<br>두 줄</div>
        		<div class="item">작은 박스</div>
        	</div>
        </body>
        
        </html>
        ```
        
    - `baseline`과 `flex-end`가 헷갈린다면?
        
        1. **flex-start**
            - 플렉스 컨테이너의 시작점(위쪽)을 기준으로 아이템들을 정렬합니다
            - 모든 아이템의 상단이 컨테이너의 상단에 맞춰집니다
            - 패딩이나 폰트 크기와 관계없이 일률적으로 위쪽에 정렬됩니다
        2. **baseline**
            - 텍스트의 기준선(baseline)을 기준으로 아이템들을 정렬합니다
            - 텍스트의 하단 라인이 서로 맞춰지도록 정렬됩니다
            - 폰트 크기가 다르더라도 텍스트가 자연스럽게 한 줄에 정렬된 것처럼 보입니다
        
        ```HTML
        <!DOCTYPE html>
        <html>
        
        <head>
        	<style>
        		.container {
        			margin-bottom: 50px;
        			background: \#f0f0f0;
        			padding: 10px;
        			height: 150px;
        			display: flex;
        		}
        
        		.baseline {
        			align-items: baseline;
        		}
        
        		.flex-start {
        			align-items: flex-start;
        		}
        
        		.item {
        			background: skyblue;
        			margin: 5px;
        			padding: 10px;
        		}
        
        		/* 각 아이템마다 다른 폰트 크기와 패딩을 적용 */
        		.text-small {
        			font-size: 12px;
        			padding-top: 5px;
        		}
        
        		.text-medium {
        			font-size: 24px;
        			padding-top: 15px;
        		}
        
        		.text-large {
        			font-size: 36px;
        			padding-top: 25px;
        		}
        
        		h2 {
        			margin: 10px 0;
        		}
        	</style>
        </head>
        
        <body>
        	<h2>align-items: baseline</h2>
        	<div class="container baseline">
        		<div class="item text-small">작은 텍스트</div>
        		<div class="item text-medium">중간 텍스트</div>
        		<div class="item text-large">큰 텍스트</div>
        	</div>
        
        	<h2>align-items: flex-start</h2>
        	<div class="container flex-start">
        		<div class="item text-small">작은 텍스트</div>
        		<div class="item text-medium">중간 텍스트</div>
        		<div class="item text-large">큰 텍스트</div>
        	</div>
        </body>
        
        </html>
        ```
        
    
    > [!important] 정확히는 `align-items`는 **세로 정렬**보단 `justify-content` 방향과 교차하는 **교차축**입니다.
    
    - `flex-direction: column`을 추가할 경우 가로 방향이기 때문입니다.
        - 예시 코드
            
            ```HTML
            <!DOCTYPE html>
            <html>
            
            <head>
            	<style>
            		.container {
            			display: flex;
            			/* flex는 이제 세로로 정렬됩니다. 이로 인해 align-items는 가로축을 기준으로 정렬합니다. */
            			flex-direction: column;
            			background: \#f0f0f0;
            			padding: 10px;
            			margin-bottom: 20px;
            			height: 200px;
            		}
            
            		.align-start {
            			/* 가로축을 기준으로 시작 지점에 정렬합니다. */
            			align-items: flex-start;
            		}
            
            		.align-center {
            			/* 가로축을 기준으로 가운데에 정렬합니다. */
            			align-items: center;
            		}
            
            		.align-end {
            			/* 가로축을 기준으로 끝 지점에 정렬합니다. */
            			align-items: flex-end;
            		}
            
            		.item {
            			background: skyblue;
            			padding: 10px;
            			margin: 5px;
            			width: 100px;
            		}
            	</style>
            </head>
            
            <body>
            	<div class="container align-start">
            		<div class="item">flex-start</div>
            		<div class="item">왼쪽 정렬</div>
            	</div>
            
            	<div class="container align-center">
            		<div class="item">center</div>
            		<div class="item">가운데 정렬</div>
            	</div>
            
            	<div class="container align-end">
            		<div class="item">flex-end</div>
            		<div class="item">오른쪽 정렬</div>
            	</div>
            </body>
            
            </html>
            ```
            

  

## 배치를 바꾸는 법 3) grid: 격자 모양 정렬하기

### 📌 **CSS Grid란?**

CSS Grid는 **2차원(행과 열) 레이아웃 시스템**입니다. 웹 페이지를 **격자(그리드) 형태**로 나누어 요소들을 배치할 수 있게 해줍니다.

**기본 용어:**

- Grid Container: 그리드의 전체 영역
- Grid Item: 그리드 내부의 각 요소

  

1. **격자 모양을 정렬하는 방법**
    
    - `display: grid`: 격자 모양으로 만들기
    - `grid-template-columns: 1fr 1fr 1fr;` : 3개의 컬럼을 만들고 각 컬럼의 길이를 동일한 비율로 설정하기
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS Grid 예제</title>
        	<style>
        		.grid-container {
        			/* 격자 모양으로 정렬 */
        			display: grid;
        			/* 3개의 열을 동일한 비율로 */
        			grid-template-columns: 1fr 1fr 1fr;
        			/* 격자 사이의 간격 */
        			grid-gap: 10px;
        			/* 격자 외부 여백 */
        			padding: 10px;
        		}
        
        		.grid-item {
        			/* 배경색 */
        			background-color: \#3498db;
        			/* 테두리 */
        			border: 1px solid #000;
        			/* 텍스트 색상 */
        			color: white;
        			/* 내부 공간 */
        			padding: 20px;
        			/* 텍스트 정렬 */
        			text-align: center;
        			/* 모서리 둥글게 */
        			border-radius: 5px;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="grid-container">
        		<div class="grid-item">1</div>
        		<div class="grid-item">2</div>
        		<div class="grid-item">3</div>
        		<div class="grid-item">4</div>
        		<div class="grid-item">5</div>
        		<div class="grid-item">6</div>
        		<div class="grid-item">7</div>
        		<div class="grid-item">8</div>
        	</div>
        </body>
        
        </html>
        ```
        

  

1. **다른 비율로 격자 모양 만들기**
    
    - `grid-row`: 세로로 얼마나 차지할지 정합니다
        - 예: `grid-row: 2 / 4` 는 2번째 줄부터 4번째 줄 '전'까지
    - `grid-column`: 가로로 얼마나 차지할지 정합니다
        - 예: `grid-column: 1 / 3` 은 1번째 칸부터 3번째 칸 '전'까지
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS Grid 예제</title>
        	<style>
        		.grid-container {
        			display: grid;
        			grid-template-columns: repeat(3, 1fr);
        			/* 3개의 열을 동일한 비율로 */
        			grid-gap: 10px;
        			/* 격자 사이의 간격 */
        			padding: 10px;
        		}
        
        		.grid-item {
        			background-color: \#3498db;
        			color: white;
        			padding: 20px;
        			text-align: center;
        			border-radius: 5px;
        		}
        
        		/* 특별한 아이템 스타일링 */
        		.item1 {
        			/* 1번째 줄부터 3번째 줄까지 차지 */
        			grid-column: 1 / 3;
        			background-color: \#e74c3c;
        		}
        
        		.item4 {
        			/* 2번째 행부터 4번째 행까지 차지 */
        			grid-row: 2 / 4;
        			background-color: \#2ecc71;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="grid-container">
        		<div class="grid-item item1">1</div>
        		<div class="grid-item">2</div>
        		<div class="grid-item">3</div>
        		<div class="grid-item item4">4</div>
        		<div class="grid-item">5</div>
        		<div class="grid-item">6</div>
        		<div class="grid-item">7</div>
        		<div class="grid-item">8</div>
        	</div>
        </body>
        
        </html>
        ```
        

## 크기를 지정하는 법 - px, em, rem

**단위 설명**

1. **px (픽셀)**
    
    - **고정된 절대 단위**
    - **화면의 실제 픽셀 단위**로 크기 지정
    - 사용자의 브라우저 설정이나 디바이스에 영향을 받지 않음
    
    - 예시 코드
        
        - `html` (루트)에 `16px`로 설정
        - `container`를 가진 `div` 태그에 `20px`로 설정
        - `px-example`를 가진 `div` 태그에 `24px`로 설정
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS 단위 비교</title>
        	<style>
        		/* 루트 요소의 기본 폰트 사이즈 설정 */
        		html {
        			/* html 전체 (루트) 폰트 사이즈를 16px로 설정 */
        			font-size: 16px;
        		}
        
        		.container {
        			/* 컨테이너 폰트 사이즈를 20px로 설정 */
        			font-size: 20px;
        			border: 1px solid \#ccc;
        			margin: 20px;
        			padding: 10px;
        		}
        
        		/* px 예시 */
        		.px-example {
        			/* px 단위 폰트 사이즈를 12px로 설정 */
        			font-size: 24px;
        			margin-bottom: 16px;
        			padding: 16px;
        			background-color: \#ffebee;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="container">
        		<div>컨테이너 크기: 20px</div>
        		<div class="px-example">
        			px 단위 예시 (24px)
        		</div>
        	</div>
        </body>
        
        </html>
        ```
        
2. **em**
    
    - **상대 단위**
    - **부모 요소**의 `**font-size**`**를 기준**으로 크기가 결정됨
    - 중첩된 요소에서는 계산이 복잡해질 수 있음
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS 단위 비교</title>
        	<style>
        		/* 루트 요소의 기본 폰트 사이즈 설정 */
        		html {
        			/* html 전체 (루트) 폰트 사이즈를 16px로 설정 */
        			font-size: 16px;
        		}
        
        		.container {
        			/* 컨테이너 폰트 사이즈를 20px로 설정 */
        			font-size: 20px;
        			border: 1px solid \#ccc;
        			margin: 20px;
        			padding: 10px;
        		}
        
        		/* em 예시 */
        		.em-example {
        			/* container가 부모 -> 20px의 2배 */
        			font-size: 2em;
        			margin-bottom: 1em;
        			padding: 1em;
        			background-color: \#e3f2fd;
        		}
        
        		.em-example2 {
        			/* container가 부모X -> html 기준 16px의 2배 */
        			font-size: 2em;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="container">
        		<div>컨테이너 크기: 20px</div>
        		<div class="em-example">
        			em 단위 예시 (2em) 20px의 2배
        		</div>
        	</div>
        	<div class="em-example2">
        		em 단위 예시 (2em) 16px의 2배
        	</div>
        </body>
        
        </html>
        ```
        
3. **rem (root em)**
    
    - **상대 단위**
    - **루트 요소(html)**의 `**font-size**`**를 기준**으로 크기가 결정됨
    - `em`과 달리 항상 일관된 크기를 유지
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS 단위 비교</title>
        	<style>
        		/* 루트 요소의 기본 폰트 사이즈 설정 */
        		html {
        			/* html 전체 (루트) 폰트 사이즈를 16px로 설정 */
        			font-size: 16px;
        		}
        
        		.container {
        			/* 컨테이너 폰트 사이즈를 20px로 설정 */
        			font-size: 20px;
        			border: 1px solid \#ccc;
        			margin: 20px;
        			padding: 10px;
        		}
        
        		/* rem 예시 */
        		.rem-example {
        			/* html(루트) 기준 -> 16px의 2배 */
        			font-size: 2rem;
        			margin-bottom: 1rem;
        			padding: 1rem;
        			background-color: \#e3f2fd;
        		}
        
        		.rem-example2 {
        			/* html(루트) 기준 -> 16px의 2배 */
        			font-size: 2rem;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="container">
        		<div>컨테이너 크기: 20px</div>
        		<div class="rem-example">
        			rem 단위 예시 (2rem) 16px의 2배
        		</div>
        	</div>
        	<div class="rem-example2">
        		rem 단위 예시 (2rem) 16px의 2배
        	</div>
        </body>
        
        </html>
        ```
        

  

## CSS를 잘 작성한다는 것

### **1. CSS 잘 작성하기 🎨** 

1. **유지보수가 쉽게 작성하기**
    
    - 일관된 네이밍 규칙 사용 (예: BEM 방식)
        
        ```CSS
        .block { ... }
        .block__element { ... }
        .block--modifier { ... }
        .block__element--modifier { ... }
        ```
        
    - 관련된 스타일을 그룹화
    - 적절한 주석 달기
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS 단위 비교</title>
        	<style>
        		/* Block */
        		.card {
        			border: 1px solid \#ddd;
        			padding: 20px;
        		}
        
        		/* Elements */
        		.card__title {
        			font-size: 20px;
        		}
        
        		.card__text {
        			color: #666;
        		}
        
        		.card__button {
        			padding: 10px 20px;
        		}
        
        		/* Modifiers */
        		.card__button--primary {
        			background: blue;
        			color: white;
        		}
        
        		.card__button--secondary {
        			background: gray;
        			color: white;
        		}
        	</style>
        </head>
        
        <body>
        	<div class="card">
        		<div class="card__content">
        			<h2 class="card__title">제목</h2>
        			<p class="card__text">내용</p>
        		</div>
        		<button class="card__button card__button--primary">확인</button>
        		<button class="card__button card__button--secondary">취소</button>
        	</div>
        </body>
        
        </html>
        ```
        
2. **재사용성 고려하기**
    
    - 공통 스타일은 별도로 관리
    
    - 너무 구체적인 스타일 피하기
        
        ```CSS
        /* ❌ 나쁜 예시: 너무 구체적인 선택자 */
        .header .navigation .nav-list .nav-item .nav-link {
            color: blue;
        }
        
        /* ✅ 좋은 예시: 단순한 선택자 */
        .nav-link {
            color: blue;
        }
        ```
        
    
    - 변수(CSS 변수) 활용하기
    
    - 예시 코드
        
        ```HTML
        <!DOCTYPE html>
        <html lang="en">
        
        <head>
        	<meta charset="UTF-8">
        	<meta name="viewport" content="width=device-width, initial-scale=1.0">
        	<title>CSS 단위 비교</title>
        	<style>
        		:root {
        			/* 색상을 변수로 설정 가능 */
        			--primary-color: \#007bff;
        			--white-color: \#fff;
        
        			--padding-size: 16px;
        		}
        
        		.card {
        			background-color: var(--primary-color);
        			color: var(--white-color);
        			padding: var(--padding-size);
        		}
        	</style>
        </head>
        
        <body>
        	<div class="card">
        		내용
        	</div>
        </body>
        
        </html>
        ```
        
3. **성능 고려하기**
    - 불필요한 선택자 줄이기
    - 중복 코드 최소화
    - 간단한 선택자 사용하기

  

### **2. 선택자 사용 가이드 🎯**

1. **클래스(class) 선택자** `**.class**`
    - ✅ 가장 권장되는 방식
    - ✅ 재사용 가능
        - 예시
            
            ```CSS
            .button {
              padding: 10px 20px;
              border-radius: 4px;
            }
            ```
            
            ```HTML
            <!-- 같은 .button 클래스를 여러 요소에 사용 가능 -->
            <button class="button">로그인</button>
            <button class="button">회원가입</button>
            <button class="button">확인</button>
            ```
            
    - ✅ 유지보수 쉬움
        - 예시
            
            ```CSS
            
            /* BAD 모든 div에 적용할 경우 헷갈림 */
            div {
                background: blue;
            }
            
            /* GOOD 한 곳에서 수정하면 card를 클래스로 가진 모든 곳에 자동 적용 */
            .card {
            		background: red;
                border: 1px solid \#ddd;
                border-radius: 4px;
                padding: 1rem;
            }
            ```
            
            ```HTML
            <div class="card">카드1</div>
            <div class="card">카드2</div>
            <div>카드3</div>
            ```
            

  

1. **ID 선택자** `**#id**`
    
    - ⚠️ 한 페이지에 하나만 사용 가능
    - ⚠️ 너무 구체적이라 재사용 어려움
    - 🔍 특별한 경우에만 사용 (예: 헤더, 푸터)
    
    ```CSS
    \#header { ... }
    \#main-navigation { ... }
    ```
    

  

1. **태그 선택자 tag**
    
    - ⚠️ 너무 광범위함
    - ⚠️ 의도치 않은 스타일 적용 가능
    - 🔍 기본 스타일 초기화할 때 주로 사용
    
    ```CSS
    body { ... }
    h1 { ... }
    ```
    
      
    

### **3. 추천하는 방식 💡**

1. **기본적으로 클래스 선택자 사용**

```CSS
.button { ... }
.button-primary { ... }
.card-title { ... }
```

  

1. **필요한 경우에만 조합해서 사용**

```CSS
/* 특정 상황에서만 필요할 때 */
.card .title { ... }
.sidebar .menu { ... }
```

  

1. **의미있는 이름 사용**

```CSS
/* Good */
.nav-item { ... }
.product-card { ... }

/* Bad */
.red-text { ... }
.big-box { ... }
```