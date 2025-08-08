useRef는 컴포넌트가 일부 정보를 “기억”해야해서 렌더링되지 않게 하는 훅입니다.

```jsx
import { useRef } from 'react';

// 컴포넌트 내부라고 가정
const ref = useRef(0);

// ref 반환 값 (객체)
{
  current: 0 // useRef에 전달한 값
}

// ref값 변경 ( 변수처럼 값을 직접 바꿈 )
ref.current = 100; // 변경해도 리렌더링 되지 않음
```

![스크린샷 2025-04-11 오후 12.46.12.png](attachment:d9610faa-52d4-4f7e-aecc-e1d409ea6de7:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.46.12.png)

- 추적하지 않는다는 건 결국 값이 바뀌었는지 감시를 안하기에 → UI 업데이트도 안함
- React는 보통 단방향 데이터 흐름을 강요해요.
    - ex. 부모 → 자식으로만 데이터 전달
    - ex. 상태는 setState로만 변경 가능
    - ex. 렌더링은 React가 결정
- but. ref는 에외입니다. React 감시망을 피해 값을 저장하고, 바꾸고 DOM까지 조작할 수 있어요

![스크린샷 2025-04-11 오후 12.45.39.png](attachment:a0b32324-c1ae-420f-a5d8-46d1f91678e0:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.45.39.png)

ref는 React의 렌더링 프로세스 외부에서 current 값을 수정할 수 있어요, 하지만 state는 setter함수를 사용하여 React가 관리하는 리렌더 대기열에 넣어야해요. 즉 refs는 변경 가능한, state는 불변이라는 특징을 가져요

React는 불변성을 추구하기에 공식 문서에서도 Ref는 예외적인 상황에서만 사용해야 한다고 해요. (’’’자주 필요하지 않은 탈출구입니다.’’’)

![스크린샷 2025-04-11 오후 12.55.06.png](attachment:e56ce8a3-15c6-4764-af59-446aaa045ecf:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.55.06.png)

여기서 말하는 내용 중 핵심은 저 코드는 React 내부 코드는 아니고 ref를 이런식으로 구현할 수 있다 정도의 코드라고 생각해주세요. 결국 ref도 결국 state처럼 React가 내부에 저장할 공간이 필요해요

ref는 { current : 초기값} 객체이고, 리렌더링 되어도 항상 같은 객체를 반환해야 해요. (그래야 값이 유지되니까) → 그렇기에 내부적으로 React는 이 객체를 초기 렌더에 한 번만 만들고, 그 다음엔 재사용합니다.

unused는 setter를 사용하지 않는다는 의미예요. (ref.current로 직접 값을 바꿀거기에 사용X)

객체지향에서 인스턴스 필드처럼 생각해도 된다는 비유를 통해서 this.value 처럼 인스턴스 변수 만들 수 있다고 표현하고 싶은 거 같아요 ( this.value === valueref.current )

---

리액트는 선언형 방식이기 때문에, 대부분 state → UI 변경을 하지만, scroll, 비디오 제어 등은 ref로 DOM을 직접 조작해야 합니다.

```jsx
import { useRef } from 'react';

//컴포넌트 내부
const inputRef = useRef(null);

//return 내부
<input type='text' ref={inputRef} /> // DOM과 연결 완료!

// DOM과 연결하면 브라우저 API를 직접 사용할 수 있습니다. 
const handleClick = () => {
    inputRef.current.focus();
  }

```

즉, useRef로 특정 DOM 노드를 직접 가리키고, 나중에 그 노드에 스크롤, 포커스, 스타일 조작 같은 브라우저 내장 기능을 사용할 수 있어요

DOM을 직접 조작하는 경우는 어떤 경우가 있나요?

- navigation에서 섹터 누를 때, smooth하게 이동시킬 때
    
    [https://cdg-portfolio.com/](https://cdg-portfolio.com/)
    
    ```jsx
    import { useRef } from 'react';
    
    export default function ScrollBox() {
      const boxRef = useRef(null);
    
      const scrollToBox = () => {
        boxRef.current.scrollIntoView({ behavior: 'smooth' });
      };
    
      return (
        <div>
          <button onClick={scrollToBox}>박스로 스크롤 이동</button>
    
          <div style={{ height: '1000px' }} />
    
          <div
            ref={boxRef}
            style={{
              width: '200px',
              height: '200px',
              backgroundColor: 'skyblue',
            }}
          >
            여기에 도착!
          </div>
        </div>
      );
    }
    ```
    

![스크린샷 2025-04-11 오후 2.14.11.png](attachment:ebf3ad25-8d8f-49b7-82ff-325b500c3448:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.14.11.png)

정적인 요소 3개일 땐

```jsx
const firstRef = useRef(null);
const secondRef = useRef(null);
const thirdRef = useRef(null);

return (
  <>
    <div ref={firstRef}>First</div>
    <div ref={secondRef}>Second</div>
    <div ref={thirdRef}>Third</div>
  </>
);
```

요런식으로 DOM과 직접 연결해주면 되지만, 리스트가 동적이면 문제가 생긴다.

```jsx
{cats.map((cat) => {
  const ref = useRef(); ❌ 훅 규칙 위반
  return <div ref={ref}>...</div>;
})}
```

그럼 어떻게 해야할까?

```jsx
// 컴포넌트 내부
const itemRefs = useRef(new Map());

<ul>
  {cats.map(cat => (
    <li
      key={cat}
      ref={(node) => {
        if (node) {
          itemRefs.current.set(cat, node);   // 생성 시 저장
        } else {
          itemRefs.current.delete(cat);      // 사라질 때 제거
        }
      }}
    />
  ))}
</ul>
```

동적으로 이름(ID) 기준으로 접근하려면 key-value 구조가 필요함, 인덱스로만 접근하면 순서가 바뀔 때 위험해서 Map 사용을 고려할 수 있다.

```jsx
import { useRef } from 'react';

export default function ScrollSections() {
  const sectionMapRef = useRef(new Map());

  const sections = ['Intro', 'About', 'Projects', 'Contact'];

  const scrollToSection = (name) => {
    const sectionNode = sectionMapRef.current.get(name);
    sectionNode?.scrollIntoView({ behavior: 'smooth' });
  };

  return (
    <div>
      {/* 네비게이션 */}
      <nav className="sticky top-0 bg-white shadow-md p-4 flex gap-4">
        {sections.map((name) => (
          <button
            key={name}
            onClick={() => scrollToSection(name)}
            className="px-4 py-2 bg-blue-500 text-white rounded"
          >
            {name}
          </button>
        ))}
      </nav>

      {/* 섹션들 */}
      <div>
        {sections.map((name) => (
          <section
            key={name}
            ref={(node) => {
              if (node) {
                sectionMapRef.current.set(name, node);
              } else {
                sectionMapRef.current.delete(name);
              }
            }}
            className="h-[100vh] flex items-center justify-center border-b-2"
          >
            <h1 className="text-4xl font-bold">{name}</h1>
          </section>
        ))}
      </div>
    </div>
  );
}
```

다른 컴포넌트의 DOM 노드 접근할 땐, ref를 props로 내려주자!

```jsx
import { useRef } from 'react';

function MyInput({ ref }) {
  return <input ref={ref} />;
}

function MyForm() {
  const inputRef = useRef(null);
  return <MyInput ref={inputRef} />
}
```

하지만 이런 방식은 부모가 자식의 DOM노드에 대한 권한을 다 갖고 있어서 위험합니다. 예시로, 멘토링 받으면서 setter함수를 props로 내려주면 자식이 부모의 상태를 마음대로 바꿀 수 있어서 위험하기에 이벤트 핸들러를 만들어서 자식으로 내려주라는 조언을 들었었습니다. 이 예시와 비슷하다고 느꼈습니다.

부모가 자식 DOM노드에 대한 권한을 전부 주게 하지 않으려면, useImperativeHandle을 사용합니다.

```jsx
import { useRef, useImperativeHandle, forwardRef } from "react";

// 자식 컴포넌트
const MyInput = forwardRef((props, ref) => {
  const realInputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus() {
      realInputRef.current.focus();
    },
  }));

  return <input ref={realInputRef} />;
});

// 부모는 ref를 넘기지만 자식은 useImperativeHandle을 통해 진짜 DOM은 숨기고 focus 메서드만 노출
// 부모는 ref.current.focus()만 사용할 수 있고, ref.current.style 같은건 못 씀
```

```jsx
export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus(); // ✅ 가능
    // inputRef.current.value = "이건 안 됨!" ❌
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
}
```

|ref 종류|부모에서 가능한 일|
|---|---|
|일반 DOM ref (ref={ref})|뭐든지 다 가능 (스타일, 값 등)|
|useImperativeHandle로 제한한 ref|자식이 노출한 기능만 가능|
|||

![스크린샷 2025-04-11 오후 3.47.12.png](attachment:289ba37c-4bf7-41f3-a06b-0c1261513b48:%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-04-11_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.47.12.png)

이 글은 React에서는 render phase, commit phase가 있는데 ref는 commit phase에서 생성되기에 앵간하면 ref.current로 접근하는 것은 이벤트 핸들러 안에서 하든, 이벤트가 없다면 useEffect 안에서 접근해라는 뜻이에요

refresh

React는 기본적으로 선언형 프레임워크처럼 동작하는 라이브러리

DOM 생성/삭제, 렌더링 순서, 상태 변화 등 다 React가 조절하지만 React가 안 도와주는게 있어요

포커스 이동, 스크롤 조정, 캔버스, 비디오, 타사 라이브러리 DOM 조작 등등 ← 이런건 ref를 써야 해요.

ref를 이용해서 DOM을 직접 수정할 땐, 주의해서 사용해야 합니다.

ref를 사용해서 DOM을 조작해도 React는 DOM이 변경된 줄 몰라서 ref.current.remove()로 DOM 요소를 지웠는데 이 삭제된 DOM을 React가 조작하려고 하면 버그가 발생할 수 있어요

| 사용 예시            | 사용 가능 여부 | 설명                            |
| ---------------- | -------- | ----------------------------- |
| input.focus()    | ✅        | 포커스 이동                        |
| scrollIntoView() | ✅        | 스크롤 이동                        |
| DOM요소에 자식 추가     | ⚠️       | 꼭 필요한 경우만, React가 안 건드는 영역에서만 |
| DOM 요소의 remove() | ❌        | React가 관리 중인 DOM은 건드리면 안됨     |