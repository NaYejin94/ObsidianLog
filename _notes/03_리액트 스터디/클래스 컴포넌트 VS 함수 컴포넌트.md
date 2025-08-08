# 🎯 함수 컴포넌트의 시초

```jsx
var Aquarium = (props) => {
	var fish = getFish(props.species)
	return <Tank>{fish}</Tank>
}

var Aquarium = ({ species }) => <Tank>{getFish(species)}</Tank>
```

## 🌟 특징

✅ **리액트 0.14버전**부터 등장한 오래된 컴포넌트 선언 방식

✅ **초기에는 “무상태 함수 컴포넌트”** → 상태 없이 정적 요소를 렌더링

✅ **클래스 컴포넌트의 render 역할만 수행**

✅ **React 16.8 이후 Hook 도입** → 이제는 상태 관리 & 생명주기 제어까지 가능

# 🏛 클래스 컴포넌트

📌 **클래스 컴포넌트 기본 구조**

```jsx
interface SampleProps {
	required?: boolean;
	text: string;
}

interface SampleState {
	count: number;
	isLimited?: boolean;
}

class SampleComponent extends React.Component<SampleProps, SampleState> {
	constructor(props: SampleProps) { //초기화되는 시점에 호출
		super(props); //상위 컴포넌트의 생성자 함수 호출
		this.state = { count: 0, isLimited: false };  //state 초기화
	}

	private handleClick = () => { //자동으로 this 바인딩
		const newValue = this.state.count + 1;
		this.setState({ count: newValue, isLimited: newValue >= 10 });
	};

	public render() {
		const { required, text } = this.props;
		const { count, isLimited } = this.state;

		return (
			<h2>
				Sample Component
				<div>{required ? "필수" : "필수 아님"}</div>
				<div>문자: {text}</div>
				<div>count: {count}</div>
				<button onClick={this.handleClick} disabled={isLimited}>증가</button>
			</h2>
		);
	}
}

```

## 📌 클래스 컴포넌트 특징

✅ class 선언 방식으로 extends React.Component 혹은 React.PureComponent 사용

✅ state를 관리하기 위해 constructor에서 초기화

✅ render() 메소드 내에서 UI 렌더링

# 🔥 **Contructor 없이 state를 초기화할 수 있을까?**

**→ 가능하다! 클래스 필드(class field) 문법 사용하면 OK!**

```jsx
class SampleComponent2 extends Component {
  state = { count: 1 };

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

✅ ES2022에서 클래스 필드 추가됨

✅ 이제 constructor 없이도 state={} 방식으로 선언 가능

✅ 코드가 훨씬 간결하고 직관적

# 🎯 일반 함수에서 this 바인딩 문제

**클래스 컴포넌트의 메소드가 일반 함수로 선언되면 this 바인딩이 필요하다.**

```jsx
class SampleComponent extends Component<Props, State> {
	private constructor(props: Props) {
		super(props);
		this.state = { count: 1 };
		
		// 🛑 일반 함수에서는 this 바인딩이 필요!
		this.handleClick = this.handleClick.bind(this);
	}

	private handleClick() {
		this.setState((prev) => ({ count: prev.count + 1 }));
	}

	public render() {
		return (
			<div>
				<button onClick={this.handleClick}>증가</button>
				{this.state.count}
			</div>
		);
	}
}
```

## 📝 **this 바인딩 문제 해결 방법**

1️⃣ constructor에서 this.handleClick = this.handleClick.bind(this)

2️⃣ 화살표 함수 사용 → private handleClick = () ⇒ {…}

# ⚡ 클래스 컴포넌트의 생명주기 메소드

### 📌 생명주기 메소드가 실행되는 시점

1️⃣ 마운트(mount): 컴포넌트가 마운팅(생성)되는 시점

2️⃣ 업데이트(update): 이미 생성된 컴포넌트의 내용이 변경(업데이트)되는 시점

3️⃣ 언마운트(unmount): 컴포넌트가 더 이상 존재하지 않는 시점

## render()

✅ 생명주기 메서드 중 하나로, 리액트 클래스 컴포넌트의 유일한 필수 값으로 항상 쓰인다.

✅ 컴포넌트가 UI를 렌더링하기 위해 쓰이며 업데이트와 마운트 과정에서 일어난다.

✅ 항상 순수해야 하며 부수 효과가 없어야 한다.

- 같은 입력값이 들어가면 항상 같은 결과물을 반환해야 한다는 뜻
- 따라서 render() 내부에서 state를 직접 업데이트하는 this.setState를 호출해서는 안된다.

## ComponentDidMount()

✅ 클래스 컴포넌트가 마운트되고 그 다음 호출되는 생명주기 메소드

✅ 이 함수는 컴포넌트가 마운트되고 준비되는 즉시 실행된다.

✅ render()와는 다르게, 이 함수 내부에서는 this.setState()로 state 값을 변경하는 것이 가능하다.

✅ 브라우저가 실제로 UI를 업데이트하기 전에 실행되어 사용자가 변경되는 것을 눈치챌 수 없게 만든다.

## ComponentDidUpdate()

✅ 컴포넌트 업데이트가 일어난 이후 바로 실행된다.

✅ 일반적으로 state나 props의 변화에 따라 DOM을 업데이트하는 등에 쓰인다.

✅ 여기서도 this.setState를 사용할 수 있다.

```jsx
componentDidUpdate(prevProps: Props, prevState: State){
	
	if(this.props.userName !== prevProps.userName){
		this.fetchData(this.props.userName);
	}
}
```

## ComponentWillUnMount()

✅ 컴포넌트가 언마운트되거나 더 이상 사용되지 않기 직전에 호출된다.

✅ 메모리 누수나 불필요한 작동을 막기 위한 클린업 함수를 호출하기 위한 최적의 위치이다.

✅ this.setState를 호출 불가능

✅ 이벤트를 지우거나 , API 호출을 취소하거나, setInterval, setTimeout으로 생성된 타이머를 지우는 등의 작업을 하는 데 유용

```jsx
componentWillUnMount() {
	window.removeEventListener("resize", this.resizeListener)
	clearInterval(this.intervalId)
}
```

## shouldComponentUpdate()

✅ state나 props의 변경으로 리액트 컴포넌트가 다시 리렌더링되는 것을 막기 위해 쓰인다.

✅ 기본적으로 this.setState가 호출되면 컴포넌트는 리렌더링을 일으키는데, 이 메소드를 활용하면 컴포넌트에 영향을 받지 않는 변화에 대해 정의할 수 있다.

✅ 일반적으로 state의 변화에 따라 컴포넌트가 리렌더링되는 것은 자연스러운 일이므로 이 메서드를 사용하는 것은 특정한 성능 최적화 상황에서만 고려해야 한다.

```jsx
shouldComponentUpdate(nextProps: Props, nextState: State){

	return this.props.title !== nextProps.title || this.state.input !== nextState.input
}
```

## 🚀 Component와 PureComponent의 차이점

### ✔️ `Component`→ state가 업데이트되는 대로 렌더링이 일어난다.

### ✔️ **`PureComponent`** → **state가 변경될 때, "얕은 비교(Shallow Compare)" 후 필요할 때만 렌더링!**

## 🙋‍♀️ 그렇다면 PureComponent가 무조건 좋을까?

### ✅ **사용하면 좋은 경우**

- `state`가 원시값(숫자, 문자열, 불리언 등) 또는 **얕은 비교가 충분한 객체**일 때
- 불필요한 렌더링을 방지하여 **성능을 최적화**하고 싶을 때

### ❌ **주의할 점**

- state가 **객체나 배열일 경우**, 내부 값이 변해도 참조가 같다면 **변화를 감지하지 못할 수 있다**.
    - 해결법 : **불변성 유지** (setState 시 새로운 객체 생성)

## static getDerivedStateFromProps()

✅ 이전에 존재했으나 이제는 사라진 componentWillReceiveProps를 대체할 수 있는 메소드

✅ render()를 호출하기 직전에 호출된다.

✅ static으로 선언되어 있어 this에 접근할 수 없다.

```jsx
static getDerivedStateFromProps(nextProps: Props, prevState: State){
//다음에 올 props를 바탕으로 현재의 state를 변경하고 싶을 때 사용
	if(props.name !== state.name) {
		return {
		//state가 이렇게 변경된다
			name: props.name
		}
	}
	
	//state에 영향을 미치지 않는다
	return null
			
}
```

## getSnapShotBeforeUpdate()

✅ componentWillUpdate()를 대체할 수 있는 메소드

✅ DOM이 업데이트되기 직전에 호출된다.

✅ 반환된 값은 componentDidUpdate로 전달된다.

✅ DOM이 렌더링되기 전에 윈도우 크기를 조절하거나 스크롤 위치를 조정하는 등의 작업을 처리하는 데 유용하다.

```jsx
getSnapshotBeforeUpdate(prevProps: Props, prevState: State){
	//props로 넘겨받은 배열의 길이가 이전보다 길어질 경우
	//현재 스크롤 높이 값을 반환한다.
	if (prevProps.list.length < this.props.list.length){
		const list = this.listRef.current;
		return list.scrollHeight - list.scrollTop;
	}
	return null;
}

// 3번째 인수인 snapshot은 클래스 제네릭의 3번째 인수로 넣어줄 수 있다.
componentDidUpdate(prevProps: Props, prevState: State, snapShot: Snapshot){
// getSnapshotBeforeUpdate로 넘겨받은 값은 snapshot에서 접근이 가능하다.
// 이 snapshot 값이 있다면 스크롤 위치를 재조정해 기존 아이템이 스크롤에서
// 밀리지 않도록 도와준다.
	if(snapshot !== null) {
		const list = this.listRef.current;
		list.scrollTop = list.scrollHeight - snapshot;
	}
}
```

## getDerivedStateFromError()

✅ 자식 컴포넌트에서 에러가 발생했을 때 호출되는 에러 메소드

✅ static 메소드로 , error를 인수로 받는다.

✅ 하위 컴포넌트에서 에러가 발생했을 경우에 어떻게 자식 컴포넌트를 렌더링할지 결정하는 용도로 제공되는 메소드이기 때문에 반드시 미리 정의해 둔 state 값을 반환해야 한다.

✅ 렌더링 과정에서 호출되는 메소드이기 때문에 부수 효과를 발생시켜서는 안된다.

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 에러 발생 시 state를 변경하여 UI 업데이트
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <h1>⚠️ Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

class BuggyComponent extends React.Component {
  render() {
    throw new Error("버그 발생!");
    return <h1>정상 렌더링</h1>;
  }
}

export default function App() {
  return (
    <ErrorBoundary>
      <BuggyComponent />
    </ErrorBoundary>
  );
}

```

## componentDidCatch

✅ 자식 컴포넌트에서 에러가 발생했을 때 실행되며, getDerivedStateFromError에서 에러를 잡고 state를 결정한 이후에 실행

✅ 두 개의 인수를 받는다

- getDerivedStateFromError와 동일한 error
- 정확히 어떤 컴포넌트가 에러를 발생시켰는지 정보를 가지고 있는 info

✅ 부수 효과를 수행할 수 있다

- render 단계에서 실행되는 getDerivedStateFromError와 다르게 커밋 단계에서 실행되기 때문

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, errorMessage: "" };
  }

  static getDerivedStateFromError(error) {
    // UI를 변경하기 위한 상태 업데이트
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // 에러 로깅 (콘솔 혹은 서버로 전송 가능)
    console.error("Error caught:", error, info);
    this.setState({ errorMessage: error.toString() });
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h1>⚠️ Something went wrong.</h1>
          <p>{this.state.errorMessage}</p>
        </div>
      );
    }
    return this.props.children;
  }
}

class BuggyComponent extends React.Component {
  render() {
    throw new Error("버그 발생! 🚨");
    return <h1>정상 렌더링</h1>;
  }
}

export default function App() {
  return (
    <ErrorBoundary>
      <BuggyComponent />
    </ErrorBoundary>
  );
}

```

---

# 🚀 클래스 컴포넌트의 한계

### 1️⃣ 데이터의 흐름을 추적하기 어렵다.

- 서로 다른 여러 메소드에서 state의 업데이트가 일어날 수 있으며, 메소드의 순서가 강제돼 있는 것이 아니기 때문에 사람이 읽기가 매우 어렵다.

### 2️⃣ 애플리케이션 내부 로직의 재사용이 어렵다.

- 공통 로직이 많아질수록 고차 컴포넌트 내지는 props가 많아지는 래퍼 지옥에 빠져들 위험성이 커진다.

### 3️⃣ 기능이 많아질수록 컴포넌트의 크기가 커진다.

- 컴포넌트 내부에 로직이 많아질수록, 내부에서 처리하는 데이터 흐름이 복잡해져 컴포넌트의 크기가 기하급수적으로 커지는 문제가 발생한다.

### 4️⃣ 클래스는 함수에 비해 상대적으로 어렵다.

- 자바스크립트 환경에서는 함수에 비해 클래스의 사용이 비교적 어렵고 일반적이지 않다.

# 🚀 함수 컴포넌트

```jsx
type SampleProps = {
	required?: boolean
	text: string
}

export function SampleComponent({required, text} : SampleProps) {
	const [count, setCount] = useState<number>(0)
	const [isLimited, setIsLimited] = useState<boolean>(false)
	
	function handleClick(){
		const newValue = count+1
		setCount(newValue)
		setIsLimited(newValue >=10 )
	}
	
	return (
	<h2>
		Sample Component
		<div>{required? "필수" : "필수 아님"}</div>
		<div>문자 : {text}</div>
		<div>count : {count}</div>
		<button onClick={handleClick} disabled={isLimited}>
		증가
		</button>
	</h2>
	)
}
```

### 특징

✅ 클래스 컴포넌트와 비교했을 때 확실히 간결해짐

✅ render 내부에서 this 바인딩을 조심할 필요가 없다.

✅ state는 객체가 아닌 각각의 원시값으로 관리되어 훨씬 사용하기가 편해졌다.

✅ return에서도 굳이 this를 사용하지 않더라도 props와 state에 접근할 수 있게 됐다.

# 🚀 함수 컴포넌트와 클래스 컴포넌트의 차이점

## 📌 생명주기 메소드의 부재

✅ 생명주기 메소드는 React.Component에서 오는 것이기 때문에 함수형 컴포넌트에서 사용할 수 없다.

✅ 함수형 컴포넌트는 componentDidMount, componentDidUpdate, componentWillUnmount를 비슷하게 구현할 수 있다.

✅ 그러나 비슷할 뿐이지 똑같다는 것은 아니다.

- useEffect 컴포넌트의 state를 활용해 동기적으로 부수 효과를 만드는 메커니즘이다.

## 📌 함수 컴포넌트와 렌더링된 값

✅ 함수 컴포넌트는 렌더링된 값을 고정하고, 클래스 컴포넌트는 그렇지 못하다.

```jsx
interface Props {
	user: string
}

//함수 컴포넌트로 구현한 setTimeout 예제
export function FunctionalComponent(props: Props){
	const showMessage = () => {
		alert("Hello" + props.user)
	}
	
	const handleClick = () => {
		setTimeout(showMessage, 3000)
	}
	
	return <button onClick={handleClick}>Follow</button>
}

//클래스 컴포넌트로 구현한 setTimeout 예제
export class ClassComponent extends React.Component<Props, {}>{
	private showMessage = () => {
		alert("Hello" + this.props.user)
}

private handleClick = () => {
	setTimeout(this.showMessage, 3000)
}

public render() {
	return <button onClick={this.handleClick}>Follow</button>
}

}
```

- **함수 컴포넌트의 `props`는 렌더링될 때 고정됨**
    
    → `setTimeout()`이 실행될 때, `props.user`의 값이 **당시 렌더링된 값으로 유지됨**
    
    → 이후 `props`가 변경되어도 `setTimeout()`이 실행될 때는 **이전 값이 사용됨**
    
- **클래스 컴포넌트는 `this.props`가 항상 최신 값을 참조**
    
    → `setTimeout()` 실행 시 `this.props.user`가 현재 값을 참조함
    
    → 따라서 `props.user`가 바뀌면, 3초 뒤에 실행될 때 최신 값이 사용됨
    

# 🏆 결론

✅ 클래스 컴포넌트가 사라지진 않겠지만, **신규 프로젝트에선 함수 컴포넌트가 주류이다**

✅ **React 16.8 이후, Hook을 활용한 함수 컴포넌트가 더욱 강력해짐**

✅ 기존 클래스 컴포넌트 코드도 이해할 수 있도록 기본 지식은 익혀두는 것이 좋다

---

# 🔥 고차 컴포넌트(Higher-Order Component,HOC)란?

<aside>

고차 컴포넌트(HOC)는 컴포넌트를 감싸서 새로운 기능을 추가하는 패턴이다.

쉽게 말해서, “컴포넌트를 감싸서 기능을 확장하는 함수” 라고 볼 수 있다.

</aside>

## 🎯 **📌 고차 컴포넌트로 감싼다는 의미란?**

👉 컴포넌트를 함수의 인자로 넘겨주고, 해당 함수가 새로운 컴포넌트를 반환하는 패턴이다.즉, 기능을 공통적으로 적용할 수 있도록 감싸는 것을 의미하고, 재사용 가능한 로직을 쉽게 공유할 수 있다.

```jsx
function withLoading(WrappedComponent) { //컴포넌트를 감싸는 역할
  return class extends React.Component {
    state = { loading: true };

    componentDidMount() {
      setTimeout(() => this.setState({ loading: false }), 2000);
    }

    render() {
      if (this.state.loading) return <h2>⏳ 로딩 중...</h2>;
      return <WrappedComponent {...this.props} />;
    }
  };
}

class UserProfile extends React.Component {
  render() {
    return <h2>👤 사용자 정보: {this.props.name}</h2>;
  }
}

// HOC로 감싸서 "로딩 기능이 추가된" 컴포넌트 생성
const UserProfileWithLoading = withLoading(UserProfile);

// 사용 예제
ReactDOM.render(<UserProfileWithLoading name="Alice" />, document.getElementById('root'));
```

📌 **설명**

- userProfile 컴포넌트를 withLoading HOC로 감싸서 “로딩 기능”을 추가했다.
- 결과적으로 처음에는 로딩중… 이 표시되고, 2초 후에 사용자 정보: Alice가 나타난다.

# 🚀 ErrorBoundary 컴포넌트란?

<aside>

ErrorBoundary(에러 경계 컴포넌트)는 React 애플리케이션에서 자식 컴포넌트에서 발생하는 에러를 감지하고, UI를 깨지지 않게 보호하는 역할을 한다. 그리고 여전히 클래스 컴포넌트로 구현되는 경우가 많은데, ErrorBoundary의 주요 생명주기 메소드(componentDidCatch, getDerivedStateFromError)가 클래스 컴포넌트에서만 동작하기 때문이다.

</aside>

## ✅ **ErrorBoundary 구현 방법 (클래스 컴포넌트)**

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  // 1️⃣ 에러 발생 시 UI 변경 (렌더링 단계에서 실행됨)
  static getDerivedStateFromError(error) {
    return { hasError: true }; // 에러 발생 시 상태 변경
  }

  // 2️⃣ 실제 에러 정보 기록 (커밋 단계에서 실행됨)
  componentDidCatch(error, errorInfo) {
    console.error("Error caught by ErrorBoundary:", error, errorInfo);
    // 예를 들어, 에러 정보를 서버로 전송 가능
  }

  render() {
    if (this.state.hasError) {
      return <h2>⚠️ 문제가 발생했습니다. 다시 시도해주세요.</h2>;
    }
    return this.props.children;
  }
}

```

📌 **설명**

- `getDerivedStateFromError(error)` → 에러 발생 시 UI 상태 변경
- `componentDidCatch(error, errorInfo)` → 에러 정보를 로깅 (서버 전송 가능)
- `this.state.hasError`가 `true`이면 **에러 메시지를 표시**하고, 그렇지 않으면 정상 UI를 렌더링

## ✅ **활용법**

### 1️⃣ 전체 애플리케이션 감싸기

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import ErrorBoundary from "./ErrorBoundary"; // 위에서 만든 ErrorBoundary 가져오기

ReactDOM.render(
  <ErrorBoundary>
    <App />
  </ErrorBoundary>,
  document.getElementById("root")
);
```

✔ **앱 전체를 감싸면, 어떤 컴포넌트에서 에러가 발생해도 전체 애플리케이션이 죽지 않는다.**

## 2️⃣ 특정 컴포넌트만 감싸기

```jsx
function App() {
  return (
    <div>
      <h1>메인 페이지</h1>
      <ErrorBoundary>
        <ProblematicComponent /> {/* 이 컴포넌트에서만 에러 감지 */}
      </ErrorBoundary>
    </div>
  );
}

```

✔ **일부 컴포넌트에서만 에러가 발생해도, 나머지 UI는 정상적으로 동작할 수 있다.**