1. useContext

- Context의 props drilling 문제를 해결하기 위해서 나온 훅
- props Drilling이란? 부모의 props를 자손의 손자의 증손자의 4대손까지 물려줄 때 코드가 읽기복잡하고 더러워지며 확인까지 매번 잘 하는지 체크해야 하는 **매우 번거로운 작업**

```jsx
<A props = {someting}>
	<B props = {someting}>//잘들어갔는지 확인
		<C props = {someting}>//잘들어갔는지 또 확인
			<D props = {someting}/>// 잘들어갔는지 또또확인
			</C>
		</B>
	</A>
```

```jsx
import React, { createContext, useContext } from "react";

// 1. 컨텍스트 생성
const DataContext = createContext(null);

// 2. 커스텀 훅 (컨텍스트 사용)
const useData = () => {
  return useContext(DataContext);
};

// 3. Provider 컴포넌트 (최상위에서 데이터 제공)
const DataProvider = ({ children }) => {
  const value = "Some Data"; // 여기에 필요한 데이터를 넣으면 됨
  return <DataContext.Provider value={value}>{children}</DataContext.Provider>;
	}; // <<-- DataContext.provider 로 값 전달 

// 4. 각 컴포넌트에서 useContext로 값 접근
const A = ({ children }) => {
  const data = useData();
  console.log("A:", data); // 데이터 확인
  return <div>A: {children}</div>;
};

const B = ({ children }) => {
  const data = useData();
  console.log("B:", data);
  return <div>B: {children}</div>;
};

const C = ({ children }) => {
  const data = useData();
  console.log("C:", data);
  return <div>C: {children}</div>;
};

const D = () => {
  const data = useData();
  console.log("D:", data);
  return <div>D: {data}</div>;
};

// 5. 전체 구조
const App = () => {
  return (
    <DataProvider>
      <A>
        <B>
          <C>
            <D />
          </C>
        </B>
      </A>
    </DataProvider>
  );
};

export default App;
```

- 여기서 2번을 주목 커스텀훅이 뭔지 자세히 바로 다음에 나오니까 2번 훅의 존재 자체만 보자.
- 2번에서 커스텀훅으로 한번 context를 받아서 다른 함수들이 사용하고 있다 이는 직접적으로 DataContext에 접근하는걸 방지하여 **의존성을 줄인것을** 볼 수 있다.
- 또한 useContext훅은 상태관리를 하지 않으며 **오로지 상태를 주입**(props를 주입)하는 API다.

1. useReducer

- useState보다 복잡한 상태관리 시스템이 필요할 때 사용
    
    ```jsx
    import React, { useReducer } from "react";
    
    // 1️⃣ 리듀서 함수 정의
    const reducer = (state, action) => {
      switch (action.type) {
        case "INCREMENT":
          return { count: state.count + 1 };
        case "DECREMENT":
          return { count: state.count - 1 };
        default:
          return state;
      }
    };
    
    // 2️⃣ 초기 상태
    const initialState = { count: 0 };
    
    const Counter = () => {
      // 3️⃣ useReducer 사용 → [state, dispatcher]를 반환하는 배열을 구조 분해 할당
      const [state, dispatcher] = useReducer(reducer, initialState);
      //state → 현재 상태 값 (initialState로 초기화됨)
      //dispatcher → 상태를 변경하기 위한 함수 (dispatch라고도 함)
      //reducer → 상태 변경 로직을 정의한 함수
      //initialState → 초기 상태 값
      return (
        <div>
          <p>Count: {state.count}</p>
          <button onClick={() => dispatcher({ type: "INCREMENT" })}>+</button>
          <button onClick={() => dispatcher({ type: "DECREMENT" })}>-</button>
        </div>
      );
    };
    
    export default Counter;
    ```
    
- `useReducer`가 실행되어 `[state, dispatcher]`를 반환
    
- `state.count`가 `0`으로 초기화됨
    
- 사용자가 버튼을 클릭하면 `dispatcher(action)`이 호출됨
    
- `dispatcher`가 `reducer`를 실행시킴
    
- `reducer`가 `state`를 업데이트한 새로운 상태를 반환
    
- 새로운 상태로 컴포넌트가 **리렌더링**됨
    
- 화면에 업데이트된 `count` 값이 표시됨
    

```jsx
import React, { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0); // 상태값을 직접 관리

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
    </div>
  );
};

export default Counter;
```

- State는 직관적이고 짧다.

|비교 항목|`useState`|`useReducer`|
|---|---|---|

|**사용 방식**|`setState(새로운 값)` 호출 상태를 직접 설정|`dispatch(action)`(액션 객체를 발생시켜 상태변경 ) 호출|
|---|---|---|

|**상태 변경 로직**|컴포넌트 내부에서 직접 처리|`reducer` 함수로 따로 관리|
|---|---|---|

|**코드 구조**|간결하고 직관적|구조화되어 있으며 예측 가능함|
|---|---|---|

|**적합한 경우**|단순한 상태 (`boolean`, `number`, `string`)|복잡한 상태 (`객체`, 여러 값 조합)|
|---|---|---|

|**가독성**|상태가 단순할 때 좋음|상태가 복잡할 때 유지보수하기 쉬움|
|---|---|---|

|**유지보수**|상태 변경이 많아지면 관리 어려움|`reducer`로 상태 변경을 따로 관리 가능|
|---|---|---|

둘다 클로저를 활용해 값을 가둬서 state를 관리한다

- closer
    
    **클로저는 반환된 내부함수가 자신이 선언됐을 때의 환경(Lexical environment)인 스코프를 기억하여 자신이 선언됐을 때의 환경(스코프) 밖에서 호출되어도 그 환경(스코프)에 접근할 수 있는 함수**
    
    ```jsx
    function makeAdder(x) {
      return function (y) {
        return x + y;
      };
    }
    
    const add5 = makeAdder(5);   // add5는 x = 5를 기억하는 함수가 됨
    const add10 = makeAdder(10); // add10은 x = 10을 기억하는 함수가 됨
    
    console.log(add5(2));  // 5 + 2 = 7
    console.log(add10(2)); // 10 + 2 = 12
    ```
    

3.1.8 useImperativeHandle

- **부모 컴포넌트가 자식 컴포넌트의 특정 메서드나 속성을 직접 제어할 수 있도록 커스텀 ref를 설정하는 데 사용**된다.
- 자식도 부모에게서 넘겨받은 REF를 원하는 대로 수정할 수 있는 훅이다

(잘 모르겠습니다 이 이상은)

3.1.9 useLayoutEffect

- useEffect와 비슷하나 DOM변경 후에 **동기적**으로 발생한다 DOM 계산 후 화면이 반영되기 전에 하고싶은 작업이 있을때 반드시 필요할 때 사용하는것이 좋다.(UI 깜빡임을 방지하거나, 레이아웃을 즉시 조정해야 할 때만)
- useEffect보다 먼저 실행된다.
- Dom업데이트 → useLayoutEffect실행 → 브라우저에 변경 사항을 반영 →useEffect실행

3.1.10 useDebugValue

- 개발하는 과정에서 디버깅 하고 싶은 정보를 이 훅에 사용
    
    - 리액트 개발자 도구의 Components영역에 출력
    - 다른 훅 내부에서만 실행가능
    
    ```jsx
    useDebugValue (data , (date)⇒`현재 시간: ${date.toISOString()}`)
    ```
    
    **2/10/2025, 3:45:30 PM"** 같은 사람이 읽기 쉬운 형식으로 표시됨.
    
    ```jsx
    useDebugValue(date);
    ```
    
    예를 들어, {} 아이콘을 눌러야 Mon Mar 12 2025 15:45:30 GMT+0900 (KST) 같은 상세 정보가 보일 수도 있음.
    
    3.1.11훅의 규칙
    
- 최상위에서만 훅을 호출한다 조건문, 반복문, 반복된 함수 내 훅 실행 x
    
- 리액트 함수 컴포넌트 사용자 정의 훅 외에는 훅을 호출할 수 없다
    
- 링크드 리스트의 호출 순서에 따라 저장된다
    
- 링크드리스트
    
    ![2234.png](attachment:2c946d98-362d-4175-8045-e2d147a26aa5:2234.png)