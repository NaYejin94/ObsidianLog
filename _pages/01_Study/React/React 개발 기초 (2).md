---
추가 일시: Invalid date
강의: React 웹 개발 시작하기
---
- [[#State]]
- [[#참조형 State]]
- [[#컴포넌트가 좋은 이유]]
- [[#컴포넌트 재사용하기]]
- [[#리액트가 렌더링하는 방식]]
- [[#인라인 스타일]]
- [[#CSS 클래스네임]]

---

# State

- 파라미터로 초기값을 전달받고 이 함수가 실행된 다음에는 배열의 형태로 요소를 리턴
- 첫 번째 요소 = state 값 (현재 변수의 값), 처음에는 useState 함수를 호출할 때 전달한 초깃값을 가지고 있다.
- 두 번째 요소 = setter 값, 함수를 호출할 때 파라미터로 전달하는 값으로 State값이 변경
- setter함수를 통해서만 값을 변경

```JavaScript
import { useState } from 'react'

const [num, setNum] = useState(1);
```

# 참조형 State

- `join` : 아규먼트로 전달한 값을 배열의 각 요소들 사이에 넣어서 하나의 문자열로 만들어주는 메소드
    - `gameHistory.join(', ')`

---

```JavaScript
const [gameHistory, setGameHistory] = useState([]);

const HandleRollClick = ()=> {
	const nextNum = random(6);
	// setNum(nextNum);
	// setSum(sum + nextNum);
	gameHistory.push(nextNum);
	setGameHistory(gameHistory);
}
```

⇒ 주석을 해제했을때는 잘 동작하던 화면이 setNum과 setSum에 주석을 처리하니 화면이 바뀌지 않는다 🥲

  

❗이유는?❗

- 주석처리를 안했을 때 → 렌더링이 일어남(화면이 변함, 기록도 함께)
    - 그 이유는? num과 sum의 state가 변하고 있기 때문!
    - ==**주의!**== ) gameHistory의 state는 변하고 있지 않다. ⇒ 배열은 참조타입이기 때문에 push를해서 값을 넣어줘도 gameHistory의 주소값이 변하지 않기 때문이다..
    - 하지만 코드들은 계속 동작하고 있다…!
        - ==console.log(gameHistory)를 하면 배열이 계속 변하고 있다…..==

  

- setNum과 setSum을 주석처리 했을 때 → 렌더링이 일어나지 않음
    
    - gameHistory라는 배열에 push로 새로운 값을 추가한다고 해도 **기존 배열의 참조값을 유지하고 있기 때문에**
    - **리액트가 state의 변경사항을 감지하지 못하고 렌더링이 발생하지 않게 됨!**
    - **[[자바스크립트의 문법과 표현]]**를 사용해서 새로운 배열을 생성하면 문제가 해결된다
    
    ```JavaScript
    setGameHistory([...gameHistory, nextNum]);
    ```
    
    - 스프레드 연산자를 쓰게 되면 HandleRollClick()이 발생할 때 마다 gameHistory의 배열을 복사하고 nextNum을 추가한 새로운 배열을 계속 생성한다
    - 새로운 배열이 생성됨에 따라 gameHistory가 참조하는 주소값이 변경되고 **따라서 리액트는 state변경을 감지할 수 있게되고 렌더링이 발생한다!!**

  

# 컴포넌트가 좋은 이유

- 반복적인 개발이 줄어든다
- 오류를 고치기 쉽다
- 일을 쉽게 나눌 수 있다.

  

# 컴포넌트 재사용하기

- `State Lifting` : 자식 컴포넌트의 State를 부모 컴포넌트로 올려주는 것

  

# 리액트가 렌더링하는 방식

- state 값이 변경되면 통째로 렌더링
- 엘리먼트를 새로 렌더링할 때 그 모습을 실제 DOM 트리에 바로 반영하는 것이 아니라 `**VIrtual DOM**`**에 반영**
    
    ⇒ 화면을 바꿀 준비만 하고 실제로는 아직 반영하지 않았음
    
- 리액트는 이때, State 변경 전의 VIrtual DOM과 변경 후의 Virtual DOM을 비교한다
- 바뀐 부분이 있으면 그걸 실제 DOM트리에 반영함

  

장점!

⇒ 개발자가 DOM노드를 신경 쓸 필요가 없다

⇒ 변경 사항들을 리액트가 적당히 모아서 처리할 수 있다.

  

# 인라인 스타일

```JavaScript
const style = {
    backgroundColor: 'pink',
}

function Button({children, onClick}) {
    return (
    <button style={style} onClick={onClick}>
        {children}
    </button>
    )
}
```

  

# CSS 클래스네임

- 자바스크립트 파일에서 CSS파일을 임포트 하는 기능

```JavaScript
import './index.css';
```

⇒ head태그 안에 style태그가 자동으로 작성된다

  

```JavaScript
import './Button.css';

function Button({children, onClick,color='blue'}) {
    const classNames = `Button ${color}`
    return (
    <button className={classNames} onClick={onClick}>
        {children}
    </button>
    )
}
```