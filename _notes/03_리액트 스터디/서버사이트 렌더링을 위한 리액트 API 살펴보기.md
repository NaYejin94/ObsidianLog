```tsx
function ChildrenComponent({fruits}:{fruits:Array<string>}){
    useEffect(()=> {
        console.log(fruits);
    },[fruits])

    function handleClick() {
        console.log('hello')
    }

    return (
        <ul>
            {fruits.map((fruit)=> (
                <li key={fruit} onClick={handleClick}>
                    {fruit}
                </li>
            ))}
        </ul>
    )
}

function SampleComponent() {
    return (
        <>
            <div>
                hello
            </div>
            <ChildrenComponent fruits={['apple','banana','peach']}/>
        </>
    )
}
```

# renderToString

- 인수로 넘겨받은 리액트 컴포넌트를 렌더링해 HTML문자열로 반환하는 함l ,o수

```tsx
const result = ReactDOMServer.renderToString(
    React.createElement('div',{id:'root'},<SampleComponent/>),
)
```

```tsx
<div id="root" data-reactroot="">
    <div>hello</div>
    <ul>
        <li>apple</li>
        <li>banana</li>
        <li>peach</li>
    </ul>
</div>
```

- **빠르게 브라우저가 렌더링할 수 있는 HTML을 제공**하는 데 목적이 있다.
    
- `data-reactroot`
    
    : 리액트 컴포넌트의 루트 엘리먼트가 무엇인지 식별하는 역할
    
    : hydrate함수에서 루트를 식별하는 기준점
    

# renderToStaticMarkup

- renderToString과 유사하게 HTML문자열을 만들지만 data-reactroot같은 리액트에서만 사용하는 추가적인 DOM속성을 만들지 않는다
- 따라서 HTML의 크기를 약간이라도 줄일 수 잇다

```jsx
const result = ReactDOMServer.renderToStaticMarkup(
    React.createElement('div',{id:'root'},<SampleComponent/>),
)
```

```jsx
<div id="root">
    <div>hello</div>
    <ul>
        <li>apple</li>
        <li>banana</li>
        <li>peach</li>
    </ul>
</div>
```

- 아무런 브라우저 액션이 없는 **정적이 내용만 필요한 경우**에 유용하다

# renderToNodeStream

||renderToString|renderToNodeStream|
|---|---|---|
|브라우저 사용|O|X|
|결과물 타입|string|ReadableStream|

- `ReadableStream` : utf-8로 인코딩된 바이트 스트림, 서버 환경에서만 사용할 수 있다.

### **renderToNodeStream이 필요한 이유?**

```tsx
;(async () => {
	const response = await fetct("...")
	
	try {
		for await (const chunk of response.body) {
			console.log('---chunk---')
			console.log(Buffer.from(chunk).toString()
		}
	}catch{
		...
	}
})()
```

```tsx
...
<!DOCTYPE html>
	<html>
		<head>
			...
		</head>
		<body>
			...
			**---chunk---**
			...
			**---chunk---**
			...
			**---chunk---**
			...
		</body>
	</html>
```

- `스트림` : 큰 데이터를 다룰 때 데이터를 청크(작은단위)로 분할해 조금씩 가져오는 방식
    
- **HTML의 크기가 매우 크다면** 일부만 먼저 생성하는 renderToNodeStream을 쓰는게 효율적
    

# renderToStaticNodeStream

- renderToStaticMarkup과 마찬가지로 리액트 자바스크립트에 필요한 **리액트 속성이 제공되지 않는다**.

# hydrate

- 생성된 HTML콘텐츠에 **자바스크립트 핸들러나 이벤트를 붙이는 역할**
    
- render 함수
    

```tsx
const rootElement = documnet.getElementById('root');
ReactDOM.render(<APP/>,rootElement)
```

⇒ 리액트를 기반으로 한 온전한 웹페이지를 만드는 데 필요한 **모든 작업을 수행**한다.

- hydrate

```tsx
const element = document.getElementById(containerId)
ReactDOM.hydrate(<App/>,element)
```

### render와 hydrate의 차이

hydrate는 기본적으로 이미 렌더링된 HTML이 있다는 가정하에 작업이 수행되고, 이 렌더링 된 HTML을 기준으로 **이벤트를 붙이는 작업만 실행**한다.

```tsx
..
<body>
	<div id="root"></div>
</body>
...

function App(){
	return <span>안녕하세요</span>
}

const rootElement = document.getElementById('root');
ReactDOM.hydrate(<App/> , rootElement) // Warning
```

⇒ 루트 내부에 아무것도 없기 때문에 경고를 보낸다!

```tsx
<body>
	<div id="root">
		<span>안녕하세요</span>
	</div>
</body>
...

const rootElement = document.getElementById('root');
ReactDOM.hydrate(<App/> , rootElement)
```

⇒ 제대로 동작한다!

⇒ data-reactroot가 없어도 동작하는 이유?

- React가 내부 HTML을 분석하고, 자동으로 `data-reactroot`를 붙인 후 hydrate 진행함.

# 예제 프로젝트

### index.html (281p)

```tsx
<html>
	<head>
		...
	</head>
	<body>
		__placeholder__
		<script src="/browser.js"></script>
	</body>
</html>
```

- `__placeholder__` : HTML 코드를 삽입하는 자리
- `browser.js` : 리액트 코드를 번들링했을 때 제공되는 리액트 자바스크립트 코드. 내부에 App.tsx, Todo.tsx, fetch가 포함되어있다. **필요한 자바스크립트 이벤트 핸들러를 붙이기 위한 코드**

### sever.ts의 루트 라우터 / (285p)

```tsx
const result = await fetchTodo()
const rootElement = createElement(
	'div',
	{id:root'},
	createElement(App, {todos:result}),
)
const renderResult = renderToString(rootElement)

const htmlResult = html.replace('__placeholder__',renderResult)
```

⇒ `renderToString`으로 리액트 컴포넌트를 HTML로 만든 뒤 placeholder에 넣어줬다.

### sever.ts의 /stream 라우터

```tsx
switch(url) {
	case : '/stream' : {
		res.setHeader('Content-Type', 'text/html')
		**res.write(indexFront)**
		
		const result = await fetchTodo()
		const rootElement = createElement(
			'div',
			{id:root'},
			createElement(App, {todos:result}),
		)
		
		**const stream = renderToNodeStream(rootElement)**
		stream.pipe(res, {end:false})
		stream.on('end', () => {
			**res.write(indexEnd)**
			res.end()
		})
		return
	}
}
```

⇒ `renderToNodeStream`은 청크단위로 결과물을 생성하기 때문에 **placeholder** 를 반으로 나누고 **청크가 생성될 때마다 res에 기록**한뒤 스트림이 종료되면 뒤의 반을 붙여 결과물을 산출한다.