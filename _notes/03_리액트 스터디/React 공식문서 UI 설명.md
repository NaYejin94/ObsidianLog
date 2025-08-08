# 조건부 렌더링

- 특정 조건에 따라 다른 UI를 렌더링하는 방법
    
- 구현방법
    

1. `if문`

```tsx
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

⇒ 유지 보수가 어렵다 🤨

⇒ 코드를 DRY하게 만들어야함

- DRY 원칙
    
    **DRY(Don't Repeat Yourself)원칙 :** **반복되는 코드(중복)를 제거**하여 코드의 **재사용성을 높이고 유지보수를 쉽게 만드는 개발 원칙**
    
    ```tsx
    <li className="item">
      {name} {isPacked && "✅"}
    </li>
    ```
    
    - AHA 원칙
        
        성급한 추상화를 피하는 방식
        
        중복을 바로 제거하지 말고, **충분한 패턴이 보일 때까지 기다렸다가 추상화 적용**
        
        <aside>
        
        "잘못된 추상화보다 중복을 선호한다"
        
        </aside>
        
    - WET 원칙
        
        초반에는 중복을 허용하고 나중에 최적화
        
    
    ⇒ **초반에는 WET 방식**을 적용하여 빠르게 개발
    
    **패턴이 명확해지면 AHA 방식**을 적용하여 적절히 추상화
    
    **유지보수성을 위해 DRY 방식**을 적용하여 중복 제거
    

1. `삼항 연산자`

```tsx
<div>{isLoggedIn ? <UserGreeting /> : <GuestGreeting />}</div>
```

1. `논리 연산자`

```tsx
{isLoggedIn && <UserGreeting />}
```

⇒ 논리 연산자의 왼쪽에 숫자를 적지 않도록 주의

&& 연산자는 **왼쪽 값이 trythy이면 그 && 이후 값을 반환**

---

# 렌더링 목록

- 여러 개의 데이터를 반복적으로 렌더링하는 방법

1. `map()`

```jsx
const items = ["React", "Vue", "Angular"];

function ItemList() {
  return (
    <ul>
      {items.map((item, index) => {
        <li key={index}>{item}</li>
      })}
    </ul>
  );
}
```

- 뭔가…뭔가…이상하다… 🤯
    
    1. **key를 인덱스로 지정하면 안되는 이유**
        
        ⇒ 배열의 요소가 삽입, 삭제, 재정령 되면 순서가 변경되기 때문에 제대로 식별할 수 없다
        
        ⇒ 1. 데이터 베이스의 고유한 키/ID 사용
        
        1. 로컬에서 고유한 ID값 생성 (uuid)
            
            → 주의! 렌더링 시마다 새로운 key가 생성되기 때문에 모든 리스트 항목을 다시 렌더링 함
            
    2. 화살표 함수 뒤에 중괄호가 붙는 경우에 반드시 **return** 사용하기
        
        ⇒ 화살표 함수는 암묵적으로 바로 뒤에 표현식을 반환하지만 블록바디를 갖는경우 return문을 작성해줘야함
        
    
    ```jsx
    import { v4 as uuidv4 } from 'uuid';
    
    const items = ["React", "Vue", "Angular"];
    
    function ItemList() {
      return (
        <ul>
          {items.map((item) => 
            <li key={uuidv4()}>{item}</li>
          )}
          /*혹은
          {items.map((item) =>{
            return <li key={uuidv4()}>{item}</li>
          })}*/
        </ul>
      );
    }
    ```
    

---

# 구성 요소를 순수하게 유지

- 컴포넌트는 `순수 함수`처럼 동작해야 함
    
    = 동일한 입력(props, state)이 주어지면 **항상 동일한 JSX를 반환해야 함**
    
    = 외부 상태를 변경하거나 부수 효과(Side Effect)를 발생시키면 안 됨
    

```jsx
let guest = 0;

function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}

=>
Tea cup for guest #2
Tea cup for guest #4
Tea cup for guest #6
```

- StrictMode
    
    **Strict Mode가 두 번 실행되는 이유**
    
    1. 순수하지 않은 코드 감지
        
        외부 변수를 변경하는 코드 감지 (guest = guest + 1;)
        
    2. 컴포넌트가 언마운트 될때 클린업 코드가 제대로 실행되는지 확인
        

```jsx
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

⇒ 외부 변수 대신 **prop에만 의존하게 하여 순수하게 유지**

### 로컬 뮤테이션

**렌더링 중에 새로 만든 변수나 객체를 변경하는 것은 OK**

```jsx
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

⇒ **전역 상태를 변경**하는 뮤테이션은 위험하다

```jsx
let cups = []; // 

export default function TeaGathering() {
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />); // 
  }
  return cups;
}
```

### 사이드 이펙트

**렌더링 과정과 직접 관련이 없는 동작**

⇒ 이벤트 핸들러는 렌더링 중에 실행되지 않고 따라서 **순수할 필요가 없다**

⇒ 일반적으로 이벤트 핸들러 내부에 속하기 때문에 허용

```jsx
function TeaGathering() {
  function handleClick() {
    alert("Let's drink tea together!"); // 사이드 이펙트
  }

  return <button onClick={handleClick}>Start Gathering</button>;
```

⇒ useEffect에 사용하는 경우 너무 자주 사용하면 성능 저하가 발생할 수 있으므로 마지막 수단으로 사용하자

---

# 트리로서의 UI

- 리액트는 UI를 트리로 모델링한다

![image.png](attachment:8be1242d-d1b4-4b00-96d2-87107581c075:image.png)

⇒ 트리는 부모-자식 관계로 이루어져 있고, **부모에서 자식으로 props를 전달함**

- 렌더 트리에서 HTML 태그는 어디에 있을까?
    
    ### **React가 HTML을 렌더링하는 과정**
    
    1. **React 렌더 트리 생성** (React 컴포넌트 트리)
    2. **React가 가상 DOM(Virtual DOM) 생성**
    3. **React DOM(Renderer)이 가상 DOM을 실제 HTML로 변환**
    4. **브라우저가 HTML을 화면에 렌더링**

### 모듈 종속성 트리

**`import` 문을 통해 어떤 모듈이 다른 모듈을 가져오는지 보여주는 트리 구조**

⇒ 번들링 과정에서 어떤 모듈이 필요한지 결정하는 데 사용

**렌더트리와의 차이점**

||**모듈 종속성 트리**|**렌더 트리**|
|---|---|---|
|**트리의 노드**|JavaScript 모듈(파일)|React 컴포넌트|
|**트리의 브랜치(연결)**|`import` 관계|컴포넌트 포함 관계|
|**비구성 요소 포함 여부**|포함|포함 안 함 (컴포넌트만)|
|**React에 직접 렌더링되는가?**|아님 (로직을 포함한 모듈도 포함)|JSX 기반으로 직접 렌더링됨|
|**주요 목적**|번들링, 모듈 관리|UI 렌더링|

![image.png](attachment:bad2600f-794f-411f-96ee-a7d836c43b94:image.png)

- 렌더 트리
    
    ```jsx
    import FancyText from './FancyText';
    import InspirationGenerator from './InspirationGenerator';
    import Copyright from './Copyright';
    
    export default function App() {
      return (
        <>
          <FancyText title text="Get Inspired App" />
          <InspirationGenerator>
            <Copyright year={2004} />
          </InspirationGenerator>
        </>
      );
    }
    ```
    
    ⇒ App.js에서 Copyright을 InspirationGenerator의 children으로 전달했기 때문에, 렌더 트리에서는 Copyright이 InspirationGenerator의 자식으로 나타남.
    

![image.png](attachment:1ad2b36f-7d67-42f6-ac19-7c0a08849f35:image.png)

- 모듈 종속성 트리
    - `App.js → InspirationGenerator.js → Copyright.js` 순서로 import 됨.
    - 파일 간 import 관계만 보여줌.

### 모듈 종속성 트리를 최적화 하는 방법

1. **트리 쉐이킹 ( Tree Shaking )**
    
    import 했지만 사용되지 않은 모듈이 있을 때
    
    → `Webpack`, `Rollup` 같은 번들러에서 자동으로 제거
    
2. **코드 스플리팅 ( Code Splitting )**
    
    너무 많은 모듈이 한꺼번에 로드될 때
    
    → `React.lazy()` 와 `Suspense`를 활용