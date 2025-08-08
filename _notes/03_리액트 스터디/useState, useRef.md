# useState

- 함수 컴포넌트 내부에서 상태를 정의하고, 이 상태를 관리할 수 있게 해주는 훅

## useState 구현 살펴보기

```jsx
import {useState} from 'react';

const [state, setState] = useState(initialState);

```

- useState의 인수로는 사용할 state의 초깃값을 넘겨준다.

### 만약 useState를 사용하지 않는다면?

```jsx
function Component() {
	let state = 'hello'

	function handleButtonClick() {
		state = 'hi'
	}
	return (
		<>
			<h1>{state}</h1> {/*버튼을 눌러도 계속 'hello'*/}
			<button onClick={handleButtonClick}>hi</button>
		</>
	)
}

```

→ 변수의 변경이 리렌더링을 유발하지 않아서 의도대로 동작하지 않는다.

```
그렇다면 변수의 변경 이후에 리렌더링을 유발한다면?

```

```jsx
function Component() {
	const [, triggerRender] = useState();
	let state = 'hello'

	function handleButtonClick() {
		state = 'hi'
		triggerRender()
	}
	return (
		<>
			<h1>{state}</h1> {/*버튼을 눌러도 계속 'hello'*/}
			<button onClick={handleButtonClick}>hi</button>
		</>
	)
}

```

→ 이래도 변함없이 계속 'hello'로 출력된다. 그 이유는 리렌더링이 발생하는 경우 함수 컴포넌트는 재실행되고 __변수 let state = 'hello'로 매번 초기화 __ 되기 때문이다.

> 그래서 useState를 사용해서 리렌더링 이전의 값을 저장하고 기억할 수 있도록 하는 것이 필요하다.

### useState의 구현 (간략한 버전)

```
function useState(initialValue) {
	let internalState = initialValue

	function setState(newValue) {
		internalState = newValue
	}

	return [internalState, setState]

const [value, setValue] = useState(0)
setValue(1)
console.log(value) // 0
}
---------------------------------------
function useState(initialValue) {
	let internalState = initialValue;

	function state() {
		return internalState
	}

	function setState(newValue) {
		internalState = newValue
	}

	return [state, setState]

const [value, setValue] = useState(0)
setValue(1)
console.log(value()) //1
}

```

자바스크립트의 클로저를 이용해서 useState 내부에 선언된 함수(setState)가 함수의 실행이 종료된 이후에도 지역변수인 state를 계속 참조할 수 있다는 것을 이용해서 상태값을 변경할 수 있도록 되어 있다.

## 게으른 초기화

```jsx
cnost [state, setState] = useState(() => {
	console.log('... 복잡한연산') // 컴포넌트가 처음 구동될 때만 실행되고, 이후 리렌더링 시에는 실행되지 않는다.
})

```

게으른 최적화는 무거운 연산이 요구될 때 사용하는 것이 좋다.

1. localStorage나 sessionStorage에 대한 접근
2. map, filter, find 같은 배열에 대한 접근
3. 초깃값 계산을 위해 함수 호출이 필요할 때

추가적인 의문 setter함수가 여러번 호출 되는 경우에 상태 업데이트는 어떻게 동작하는가?

# useEffect

useEffect는 애플리케이션 내 컴포넌트의 여러 값들을 활용해 동기적으로 부수 효과를 만드는 메커니즘, 그리고 이 부수 효과가 '언제'보단 어떤 상태값과 함께 실행되는지 살펴보는 것이 중요하다.

## 일반적인 형태

```jsx
function Component() {
// ...
	useEffect(() => {      //실행할 부수효과 콜백전달
		// do something
	}, [props, state])     //의존성 배열 전달
}

```

useEffect는 렌더링 할 때 마다 의존성 배열에 있는 값을 보면서 이 의존성의 값이 이전과 다른 게 하나라도 있으면 부수효과를 실행하는 함수이다.

```jsx
const { useState } = require("react");

function Component() {
	const [counter, setCounter] = useState(0);

	function handleClick() {
		setCounter((prev) => prev + 1);
	}
	useEffect(() => {
		console.log(counter)
		}, [counter])

return (
	<>
		<h1>{counter}</h1>
		<button onClick={handleClick}>+</button>
	</>
);
}

```

버튼을 클릭한다고 하면 다음과 같이 작동한다고 볼 수 있다.

```jsx
function Component() {
	const counter = 1

	console.log(counter)

return (
	<>
		<h1>{counter}</h1>
		<button onClick={handleClick}>+</button>
	</>
);
}

```

useEffect는 지켜보던 counter값이 변했으므로 실행된다.

결론적으로 useEffect는 state와 props의 변화 속에서 일어나는 렌더링 과정에서 실행되는 부수 효과 함수라고 볼 수 있다.

## 클린업 함수의 목적

클린업 함수는 useEffect의 콜백이 실행되기 전에 이전의 상태를 정리하기 위해 사용됩니다. 보통 메모리 누수 방지, 중복 실행 방지, 리소스 관리를 보장합니다.

클린업 함수의 실행 순서

- **최초 마운트**: 클린업 함수는 실행되지 않고, 이펙트만 실행됩니다.
- **의존성 변경**: 클린업 → 새 이펙트 순으로 실행.
- **언마운트**: 클린업만 실행.

```jsx
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('이펙트 실행: count =', count);

    return () => {
      console.log('클린업 실행: count =', count);
    };
  }, [count]);

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}

export default Counter;

```

추가적인 의문 여러 useEffect를 사용할 경우 클린업 함수는 마운트시의 반대로 실행된다. 그 이유는 무엇일까?

```jsx
import React, { useEffect } from 'react';

function NestedDependency() {
  useEffect(() => {
    // 부모 리소스: DOM 요소 생성
    const container = document.createElement('div');
    document.body.appendChild(container);
    console.log('1. 부모: 컨테이너 생성');

    return () => {
      console.log('1. 부모 클린업: 컨테이너 제거');
      document.body.removeChild(container);
    };
  }, []);

  useEffect(() => {
    // 자식 리소스: 컨테이너에 이벤트 추가
    const container = document.querySelector('div');
    const handleClick = () => console.log('컨테이너 클릭');
    container.addEventListener('click', handleClick);
    console.log('2. 자식: 이벤트 리스너 추가');

    return () => {
      console.log('2. 자식 클린업: 이벤트 리스너 제거');
      container.removeEventListener('click', handleClick);
    };
  }, []);

  return <div>계층적 의존성 테스트</div>;
}

```

## 의존성 배열

의존성 배열에 따라 useEffect는 다른 동작을 한다.

1. 빈 배열로 두는 경우 최초 렌더링 직후에 실행된 다음부터는 더 이상 실행되지 않는다
2. 의존성 배열 자체를 전달하지 않는 경우 ([]도 전달하지 않는다.) 렌더링이 발생할 때마다 실행된다.
3. 의존성 배열에 의존성을 추가할 경우 의존성이 변경될 때마다 실행된다.

위의 2번의 경우, 렌더링마다 실행되는데 이 때 useEffect없이 써도 되는 것 아닌가?

```jsx
//1
function Component() {
	console.log('렌더링됨')
}
//2
function Component() {
	useEffect(() => {
		console.log('렌더링됨')
	})
}

```

위의 두 코드의 차이점

1. 서버 사이드 렌더링 관점에서 useEffect는 클라이언트 사이드에서 실행되는 것을 보장한다. useEffect 내부에서는 window 객체의 접근에 의존하는 코드를 사용해도 된다.
2. useEffect는 컴포넌트 렌더링의 부수 효과, 즉 **컴포넌트의 렌더링이 완료된 이후에 실행된다.** 1번의 경우엔 컴포넌트가 렌더링 되는 도중에 실행된다. 따라서 2번과는 달리 서버 사이드 렌더링의 경우에 서버에서도 실행되게 된다. 또한 함수 컴포넌트의 반환을 지연시키는 행위다.

결론적으로 useEffect의 effect는 컴포넌트의 사이드 이펙트, 부수 효과를 의미한다. 컴포넌트가 렌더링된 후에 어떠한 부수 효과를 일으키고 싶을 때 사용하는 훅이다.

## useEffect의 구현 -생략

## useEffect를 사용할 때 주의할 점

- elsint-disable-line react-hooks/exhaustive-deps 주석은 최대한 자제하라
    - 위 esllint 룰은 useEffect 인수 내부에서 사용하는 값 중 의존성 배열에 포함돼 있지 않은 값이 있을 때 경고를 발생시키는 룰이다. 위 주석을 통해 경고를 무시할 수 있다.

```jsx
useEffect(() => {
	console.log(props) //props가 안에서 사용되는데 의존성 배열엔 props를 전달하지 않았다.
}, []) //elsint-disable-line react-hooks/exhaustive-deps

```

- 위의 방식대로 작성하게 된다면 props와 useEffect의 부수효과 별개로 작동하게 된다느 것을 의미하는데 이렇게 작성한다면 useEffect의 의미와 멀어질 수 있고, 버그의 가능성이 커진다. 다른 적절한 위치에서 useEffect를 호출할 수 있도록 고민해봐야 한다.

### useEffect의 첫 번째 인수에 함수명을 부여하라

```jsx
useEffect(
	function logActiveUser() {
		logging(user.id)
	},
	[user.id]
)

```

의미를 명확히 하고 가독성을 높일 수 있고, useEffect의 책임을 최소한으로 좁힐 수 있다.

### 거대한 useEffect를 만들지 마라

의존성 배열을 바탕으로 렌더링 시 의존성이 변경될 때마다 부수 효과를 실행하게 되는데 이 때 부수 효과의 크기가 커질수록 애플리케이션 성능에 악영향을 미친다. 만약 부득이하게 큰 useEffect를 만들어야 한다면 적은 의존성 배열을 사용하는 여러 개의 useEffect로 분리하는 것이 좋다. 의존성 배열이 변경될 때마다 다시 실행되는 함수이기 때문에, 부득이한 경우 최적화를 해서 담아두자.

### 불필요한 외부 함수를 만들지 마라

useEffect 내에서 사용할 부수 효과라면 내부에서 만들어서 정의해서 사용하는 편이 도움된다.

```jsx
useEffect(() => {
	const getDonationListData = async () => {
		const { list } = await fetchGetDonations();
		setDonations(list);
		return;
	};
getDonationListData();
}, []);

```

비동기 함수를 바로 넣을 수 없는 이유

- 렌더링의 예측 가능성을 보장하기 위해서

추가적인 의문:

### 왜 내부에서 비동기 실행이 괜찮은가?

- **분리된 실행**: useEffect 콜백은 동기적으로 끝나고, 비동기 작업(fetchData)은 자바스크립트 이벤트 루프에서 별도로 처리됩니다. 리액트는 이펙트 실행이 끝난 후 즉시 다음 단계(예: 클린업 등록, 렌더링)를 진행할 수 있습니다.
- **클린업 보장**: useEffect가 클린업 함수를 즉시 반환하므로, 리액트는 의존성 변경이나 언마운트 시 클린업을 정상적으로 실행할 수 있습니다.
- **예측 가능성**: 비동기 작업이 useEffect의 실행 완료를 지연시키지 않으므로, 리액트의 상태와 렌더링 흐름이 일관되게 유지됩니다.

# useMemo

비용이 큰 연산에 대한 결과를 저장해 두고, 이 저장된 값을 반환하는 훅이다.

```jsx
import { useMemo } from 'react'

const memoizedValue = useMemo(() => expensiveComputation(a, b), [a, b])
// 첫번째 인수 어떠한 값을 반환하느 생성 함수
// 두번째 인수 해당 함수가 의존하는 값의 배열을 전달

```

- > 렌더링 발생 시 의존성 배열의 값이 변경되지 않았으면 함수를 재실행하지 않고 이전에 기억해 둔 해당 값을 반환하고, 의존성 배열의 값이 변경됐다면 첫 번째 인수의 함수를 실행한 후에 그 값을 반환하고 그 값을 다시 기억한다. -> 단순히 값뿐만 아니라 컴포넌트도 가능하다.
    

# useCallback

useCallback은 인수로 넘겨받은 콜백 자체를 기억하는 훅이다. useCallback은 특정 함수를 새로 만들지 않고 다시 재사용한다는 의미이다.

```jsx
const ChildComponent = memo(({ name, value, onChange }) => {
	useEffect(() => {})
})

```

사용예시

```jsx
import React, { useState, useCallback, useEffect } from 'react';

function EffectWithCallback() {
  const [count, setCount] = useState(0);

  // 이벤트 핸들러 메모이제이션
  const handleResize = useCallback(() => {
    console.log('창 크기 변경, count:', count);
  }, [count]); // count에 의존

  useEffect(() => {
    window.addEventListener('resize', handleResize);
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, [handleResize]); // handleResize가 의존성

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}

export default EffectWithCallback;

```

```jsx
import React, { useState, useEffect } from 'react';

function InfiniteLoopImmediate() {
  const [count, setCount] = useState(0);
//무한루프 발생
  const updateCount = () => {
    setCount(count + 1); // 상태 업데이트
  };
//무한루프를 방지하기 위해 useCallback으로 참조 변경을 막는다.
const updateCount = useCallback(() => {
	setCount(count + 1);
}, [count]);

  useEffect(() => {
    updateCount(); // 즉시 실행
    return () => {
      console.log('클린업');
    };
  }, [updateCount]); // updateCount가 의존성

  return <div>카운트: {count}</div>;
}

```

### useCallback을 사용해야 하는 경우

1. 함수가 다른 객체의 의존성으로 사용될 때
    1. 함수가 다른 훅이나 컴포넌트의 의존성 배열에 포함될 때, 함수 참조가 변경되면 의존성이 불필요하게 바뀌는 것으로 간주되어 함수가 매 리렌더링마다 새로 생성되어 반복실행이나 리렌더링을 유발하는 경우가 있습니다. 이렇게 함수에 useCallback을 사용하여 참조가 변경되는 것을 막아 예방해야 합니다.

```jsx
const handleResize = useCallback(() => { /* ... */ }, [deps]); // deps 변경 시에만 재생성
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, [handleResize]); // handleResize가 안정적이어야 함

```

1. 함수가 성능에 민감한 컨텍스트로 전달될 때
    1. React.memo로 최적화된 자식 컴포넌트에 이벤트 핸들러를 props로 전달하는 경우가 있습니다. 이때 부모 컴포넌트가 리렌더링 될 때 함수또한 새로 생성되어 props로 전달되는 함수또한 변경되게 되어 최적화된 컴포넌트에 리렌더링을 유발하게 됩니다.

```jsx
const Child = React.memo(({ onClick }) => <button onClick={onClick}>클릭</button>);
const Parent = () => {
  const onClick = useCallback(() => console.log('클릭'), []); // 고정된 참조
  return <Child onClick={onClick} />;
};

```

1. 함수가 상태 변경을 유발하며 순환 의존성을 만들 때
    1. 함수가 상태를 업데이트하고, 그 상태가 함수 정의에 영향을 주며, 함수가 useEffect에 의존하는 경우 무한루프가 발생할 수 있습니다.

```jsx
import React, { useState, useEffect } from 'react';

function InfiniteLoopExample() {
  const [count, setCount] = useState(0);

  // useCallback 없이 정의
  const updateCount = () => {
    setCount(count + 1); // 상태 변경
  };
  //무한루프 해결법
  //const logCount = useCallback(() => {
  //  console.log('count:', count);
  //}, [count]);

  useEffect(() => {
    updateCount(); // 즉시 실행
    return () => {
      console.log('클린업');
    };
  }, [updateCount]); // updateCount가 의존성

  return <div>카운트: {count}</div>;
}

```

# useRef

useRef는 useState와 동일하게 컴포넌트 내부에서 렌더링이 일어나도 변경 가능한 상태값을 저장한다는 공통점이 있다. 그러나 useState와 구별되는 큰 차이점 두 가지를 가지고 있다.

1. useRef는 반환값인 객체 내부에 있는 current로 값에 접근 또는 변경할 수 있다.
2. useRef는 그 값이 변하더라도 렌더링을 발생시키지 않는다.

## useRef가 필요한 이유

let을 컴포넌트 외부에 선언해서 이용하는 방법을 살펴보자.

```jsx
let value = 0

function Component() {
	function handleClick() {
		value += 1
	}
	//...
}

```

- > 이 방식의 단점
    

1. 이 방식은 컴포넌트가 실행되어 렌더링되지 않았음에도 value라는 값이 기본적으로 존재하게 된다. 메모리에 불필요한 값을 갖게하는 악영향
2. 같은 컴포넌트가 여러 번 생성된다면 각 컴포넌트에서 가리키는 값이 모두 value로 동일하게 된다. 보통은 컴포넌트 인스턴스 하나당 하나의 값을 필요로 하기에 단점이 될 수 있다.

- > useRef는 위의 두 가지 문제를 모두 극복할 수 있는 리액트식 접근법으로 컴포넌트가 렌더링될 때만 생성되며, 컴포넌트 인스턴스가 여러 개라도 각각 별개의 값을 바라본다.
    

## 사용예시

1. 기본적인 구조와 명심할 점

```jsx
function RefComponent() {
	const inputRef = useRef()
	//렌더링이 실행되기 전이므로 undefined를 반환한다.
	console.log(inputRef.current) //undefined

	useEffect(() => {
		console.log(inputRef.current) //<input type="text"></input>
		}, [inputRef])

	return <input ref={inputRef} type='text'/>
}

```

1. 렌더링을 발생시키지 않고 원하는 상태값을 저장할 수 있다는 특징을 활용해 useState의 이전 값을 저장하는 usePrevious() 같은 훅을 구현할 때

```jsx
function usePrevious(value) {
	const ref = useRef()
	useEffect(() => {
		ref.current = value
	}, [value]) // value가 변경되면 그 값을 ref에 넣어둔다
	return ref.current
}

function SomeComponent() {
	const [counter, setCounter] = useState(0)
	const previousCounter = usePrevious(counter)

	function handleClick() {
		setCounter((prev) => prev + 1)
	}

	return (
		<button onClick={handleClick}>
			{counter} {previousCounter}
		</button>
	)

	// 0 (undefined)
	// 1, 0
	// 2, 1
	// 3, 2
}

```

1. DOM 요소에 접근 할 때

```jsx
import React, { useRef } from 'react';

function FocusInput() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current.focus(); // DOM 요소에 직접 접근
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="여기에 입력" />
      <button onClick={handleFocus}>포커스</button>
    </div>
  );
}

export default FocusInput;

```

1. 가변적인 값 유지하기 (타이머 ID 관리)

```jsx
import React, { useRef, useEffect } from 'react';

function Timer() {
  const timerRef = useRef(null);

  useEffect(() => {
    timerRef.current = setInterval(() => {
      console.log('타이머 실행 중');
    }, 1000);

    return () => {
      clearInterval(timerRef.current); // 클린업 시 사용
      console.log('타이머 정리');
    };
  }, []);

  return <div>타이머가 실행 중입니다.</div>;
}

export default Timer;

```

질문

1. 나예진 : 209~210페이지에 useMemo를 사용한 컴포넌트 메모제이션을 할때 React.memo를 사용하는 것이 더 적절한 이유가 궁금합니다
    - 답변: 일단 두 가지 방법 모두 동작은 원할히 합니다. 허나 그 구현 방식에 있어 차이가 존재하기에 React.memo가 좀 더 적절하다고 안내하는 것 같습니다.
    - React.memo는 고차 컴포넌트로서 감싼 컴포넌트의 props가 변경되지 않는 경우 리렌더링을 방지합니다. useMemo의 경우엔 hook의 종류로 최상단에 선언되어야 하고 후술되겠지만 사용하는 방식이 조금 번거로운 면이 있고 기존 컴포넌트 호출방식과 다르게 작성해야 하는 점이 컴포넌트를 메모이제이션 할 때 부적절하게 여겨집니다.
2. 이나경 : 199쪽에 나온 클린업 함수에서 클린업 함수가 항상 이전 값을 참조하는 이유가 무엇인지 궁금하고, 최신 값을 사용하려면 어떻게 해야하는지 궁금합니다.
    - 클린업 함수는 의존성의 업데이트나 변경 이후에 실행되기로 예약된 함수 입니다. 다른 의미론 useEffect내의 이펙트 부분의 부수효과를 정리하기 위한 함수라고 할 수 있습니다.
    - useEffect의 존재의미는 컴포넌트 외부의 부수효과를 처리하기 위해 사용되는 hook으로 컴포넌트의 생명주기와 관계가 없는 작동을 유발시키는 데 사용됩니다. 예를 들어 fetch로 서버의 데이터를 불러오는 동작, window 전역객체나 다른 DOM요소에 이벤트 핸들러를 동작하는 동작 들이 있습니다. 이러한 이펙트들의 부수
    
3. 최권진 : useEffect에서 클린업 함수를 실행하지 않아 이벤트 리스너가 계속 등록되면 어떤 문제가 발생하나요?
    - 메모리 누수와 같은 문제가 발생하거나 동일한 이벤트 핸들러가 여러번 실행되는 문제가 발생할 수 있습니다.