<aside> 💡

가상 DOM이 무엇인지?

실제 DOM에 비해 어떤 이점이 있는지

</aside>

## DOM과 브라우저 렌더링 과정

`DOM` : 웹페이지에 대한 인터페이스, 브라우저가 웹페이지의 콘텐츠와 구조를 **어떻게 보여줄것인가에 대한 정보**를 담고 있다.

---

브라우저의 웹사이트 접근 요청과 렌더링 과정

```jsx
#text {
	background-color: red;
	color: white;
}
<!DOCTYPE html>
<html>
	<head>
		<link rel="stylesheet" type="text/css" href="./style.css" />
		<title>Hello React!</title>
	</head>
	<body>
		<div style="width: 100%">
			<div id="text" style="width: 50%">Hello world!</div>
		</div>
	</body>
</html
```

1. HTML파일 다운로드
2. HTML 파싱 후 DOM트리 생성과 CSS파일 다운로드
3. CSS파싱 후 CSSOM 생성
4. 사용자 눈에 보이지 않는 노드는 제외하고 DOM노드를 순회한다
5. 전단계에서 방문한 노드에 대한 CSSOM 정보를 노드에 적용한다.
    - DOM노드에 스타일 정보를 적용하는 과정
        - 레이아웃 : 각 노드가 브라우저 화면의 어느 좌표에 나타나야 하는지 계산, 반드시 페인팅 과정이 동반된다
        - 페인팅 : 유효한 모습을 그리는 과정 (색 등)
6. 렌더링

---

## 가상 DOM의 탄생 배경

- **`실제 DOM`을 직접 조작하는 방식의 문제**
    
    - DOM 조작 **비용**이 많이든다
        - 웹페이지에서 요소를 변경할 때, 레이아웃이 일어나고 반드시 리페인팅이 발생하게 되므로 더 많은 비용을 브라우저와 사용자가 지불하게 된다.
        - 싱글페이지 애플리케이션에서는 추가 렌더링 작업이 더 많이 일어나게 된다.
    - 트리 구조의 **비효율성**
        - DOM은 트리구조이므로, 한 부분을 변경해도 전체 DOM이 영향을 받을 수 있다.
- **`가상 DOM`을 이용한다면…**
    
    - 메모리에서 가상 DOM을 관리하고 실제 변경에 대한 준비가 완료됐을 때 실제 DOM에 반영한다.
        
        → 변경 사항을 비교하여 최소한의 연산으로 실제 DOM을 갱신하기 때문에 렌더링 과정을 최소화 할 수 있다.
        

<aside> 💡

가상 DOM은 불필요한 렌더링을 줄여 성능을 최적화 하기 위해 탄생했다.

</aside>

---

## 리액트가 가상 DOM을 관리하는 방법

리액트는 가상 DOM을 관리하고 렌더링 과정의 최적화를 위해 **리액트 파이버**를 이용한다.

### **리액트 파이버**

- 리액트에서 관리하는 자바스크립트 객체
- **파이버 재조정자**를 통해 가상 DOM과 실제 DOM을 비교해 차이가 있으면 변경된 파이버를 기준으로 렌더링을 요청한다.

### 리액트 파이버의 특징

- 비동기적 렌더링이 일어난다
- 렌더링 작업을 작은 단위로 나누어서 실행하고 우선순위를 매긴다
- 작업 중단 및 재개를 할 수 있다.
- 컴포넌트가 최초로 마운트되는 시점에 생성되어 재사용된다.

---

## 파이버의 주요 속성

|속성|설명||
|---|---|---|
|tag|해당 노드가 어떤 타입인지 나타냄 (함수 컴포넌트, 클래스 컴포넌트, HTML 태그 등)||
|파이버는 하나의 element에 하나가 생성되는 1:1 관계|||
|stateNode|파이버 자체에 대한 참조 정보||
|child|첫 번째 자식 파이버|하나의 child만 존재|
|sibling|같은 부모를 가진 다음 형제 파이버||
|return|부모 파이버 노드||
|index|형제들 사이에서 자신의 위치||
|pendingProps|아직 작업을 미처 처리하지 못한 props (업데이트 될 예정인 props)||
|memoizedProps|pendingProps를 memoizedProps로 저장해 관리 ( 현재 적용된 props)|렌더링이 완료된 이후|
|memoizedState|함수 컴포넌트의 훅 목록 저장||
|updateQueue|해당 파이버에서 발생한 업데이트 정보|상태 업데이트, 콜백함수, DOM 업데이트|

---

## 리액트 파이버 트리

- **current 트리** : 현재 모습을 담은 트리
    
- **workInProgress 트리** : 작업 중인 상태를 나타내는 트리
    
- **더블 버퍼링** : UI 업데이트를 부드럽게 처리하기 위한 트리 구조 활용 기법
    
    - 불완전한 트리를 보여주지 않기 위해 사용
    - UI 업데이트가 필요할 때, **WorkInProgress 트리에서 변경 사항을 먼저 적용**한 후, 모든 작업이 끝나면 **한 번에 Current 트리로 교체한다.**

더블 버퍼링 예시

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1 증가</button>
    </div>
  );
}
```

1. current 트리

```jsx
Fiber(App)
 ├── Fiber(div)
 │     ├── **Fiber(p)  (현재 count: 0)**
 │     ├── Fiber(button)
```

1. WorkInProgress 트리

```jsx
Fiber(App)
 ├── Fiber(div)
 │     ├── **Fiber(p)  (새로운 count: 1)  ← 업데이트 반영됨!**
 │     ├── Fiber(button)
```

1. WorkInProgress 트리가 current트리로 교체

추가로 더블 버퍼링이 발생하는 경우

```jsx
Current(현재 UI)
 |
 |-- WorkInProgress(1번)
 
```

```jsx
업데이트 완료:
**Current( ← WorkInProgress(1번)에서 변경된 내용)**
 |
 |-- (WorkInProgress(1번)
```

```jsx
Current(현재 UI)
 |
 |-- WorkInProgress(2번)
 
```

---

## 파이버의 작업 순서

- **`beginWork()`, `completeWork()`, `commitWork()`의 개념** ⇒ 파이버 트리에서 각 작업 단위를 처리하는데 사용되는 함수
    
    - **beginWork()**
        - 각 노드에 대한 렌더링 작업을 시작하는 함수
        - 렌더링이 시작되는 시점에 호출
        - 파이버 트리 순회 중 업데이트된 상태나 props에 따라 각 노드를 업데이트 할지 말지 결정
    - **completeWork()**
        - beginWork()가 완료된 후에 호출되어 해당 노드에 대한 렌더링 작업을 마무리하는 함수
        - 자식 컴포넌트에 대한 작업을 결정
    - **commitWork()**
        - 가상 DOM에서 실제 DOM으로 변경 사항을 반영하는 함수
        - 모든 업데이트가 완료된 후 최종적으로 변경사항을 커밋하는 작업을 수행

### 파이버 노드의 생성 흐름

1. `beginWork()` 함수를 실행해 파이버 작업을 수행 (자식 노드가 끝날 때 까지)
2. `completeWork()` 함수를 실행해 파이버 작업을 완료
3. 형제가 있다면 형제로 넘어감
4. return 으로 돌아가 자신의 작업이 완료됐음을 알림

```jsx
<A1>
	<B1>안녕하세요!</B1>
	<B2>
		<C1>
			<D1 />
			<D2 />
		</C1>
	</B2>
	<B3 />
</A1>

```

➡️ 작업순서

1. A1의 beginWork() 수행
    1. 자식노드가 있으므로 B1으로 이동
2. B1은 자식노드가 없으므로 completeWork() 실행
    1. B1에 형제가 있으므로 B2로 이동
3. B2의 beginWork() 수행

…

1. B3의 completeWork()가 수행되면 상위로 올라간다
2. A1의 completeWork()가 수행
3. 루트 노드가 완성되는 순간 최종적으로 commitWork()가 수행되고 변경사항이 DOM에 반영된다

⚠️ 만약 B2에서 업데이트가 발생한다면….

1. 현재 current 트리에서 작업중…
2. B2에서 `beginWork()`를 수행하는 순간 업데이트를 감지
3. 새로운 `WorkInProgress트리`가 생성됨
4. 진행중이던 current트리는 중단하고 WorkInProgress 트리를 기반으로 진행 (A1의 completeWork()가 수행될 때 까지)

⇒ 재사용성이 높아지게 된다..!

---

## 파이버와 가상 DOM

<aside> 💡

파이버 ≠ 가상 DOM

</aside>

- **가상 DOM**
    
    - 실제 DOM을 조작하기 전에 메모리 내에서 상태를 미리 표현한 가상 구조
    - 웹 애플리케이션에서만 통용되는 개념
- **파이버**
    
    - 가상 DOM 트리의 각 노드가 무엇을 해야 하는지에 대한 정보를 담고 있는 객체
    - 렌더링 과정에서 작업을 효율적으로 나누고 우선순위를 매기는 역할

---

**번외) 렌더링 과정 종합적으로 살펴보기**

```jsx
import React, { useState } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h1>Current Count: {count}</h1>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
}

export default MyComponent;
```

1. Increment버튼을 누르면 `setCount`가 실행되어 count의 **상태가 변경**
2. 리액트가 **새로운 가상 DOM을 생성**
3. 파이버가 상태 변경을 추적하고 가상 DOM에 반영하기 위해 리액트가 **파이버 트리에서 해당 컴포넌트를 업데이트**
    1. 각 노드에서 `beginWork` 단계를 수행함
    2. 이 과정에서 `workinInProgress 트리`가 생성되고 가상 DOM에서 업데이트된 count값을 반영
    3. 모든 노드에 대해 `completeWork`가 수행되면 가상 DOM에 변경 사항을 반영
4. **가상 DOM과 실제 DOM 비교**
    1. diffing과정이 발생해 최소한의 차이만 실제 DOM에 반영되도록 준비
5. `commitWork()` 수행
6. 실제 DOM노드에 변경 내용이 반영되고 렌더링 완료