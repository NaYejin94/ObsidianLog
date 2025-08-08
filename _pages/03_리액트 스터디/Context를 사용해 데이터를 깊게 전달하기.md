왜 사용해야 하는가?

부모컴포넌트에서 자식컴포넌트로 props를 전달할 때 너무 많이 거치면 좋지않음 얼마나깊든 Context 사용하면 좋음

```jsx
<A props = {someting}>
	<B props = {someting}>//잘들어갔는지 확인
		<C props = {someting}>//잘들어갔는지 또 확인
			<D props = {someting}/>// 잘들어갔는지 또또확인
			</C>
		</B>
	</A>
	
	//기억나시죠?? 제가한거 제가 한거라 전 기억납니다
	//https://www.notion.so/Deep-Dive-1a7b8f3dcf4e80f88a7bee9450eb5c77?p=1b2b8f3dcf4e81a8b168e944a2b4a81d&pm=s
```

1. context 생성

```jsx
//levelcontext.js
- import { createContext } from 'react';
- export const LevelContext = createContext(1);
```

1. context 사용

```jsx
//Heading.js

import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Heading({ level, children }) {
  // ...
}

export default function Heading({ children }) { //<<--- level props가 삭제되고 levelContext 가져
  const level = useContext(LevelContext);
  // ...
}

//나머지 채우기 
export default function Heading({ children }) {
  const level = useContext(LevelContext);
  switch (level) {
    case 1:
      return <h1>{children}</h1>;
    case 2:
      return <h2>{children}</h2>;
    case 3:
      return <h3>{children}</h3>;
    case 4:
      return <h4>{children}</h4>;
    case 5:
      return <h5>{children}</h5>;
    case 6:
      return <h6>{children}</h6>;
    default:
      throw Error('Unknown level: ' + level);
  }
}
```

1. **Context 제공하기**

```jsx
//section.js
import { LevelContext } from './LevelContext.js';

export default function Section({ level, children }) {
  return (
    <section className="section">
      <LevelContext value={level}>
        {children}
      </LevelContext>
    </section>
  );
}
```

1. `level` prop 을 `<Section>`에 전달합니다.
    
2. `Section`은 자식을 `<LevelContext.Provider value={level}>`로 감싸줍니다.
    
3. `Heading`은 `useContext(LevelContext)`를 사용해 가장 근처의 `LevelContext`의 값을 요청합니다.
    
4. `level`이라는 prop을 `<Section>` 컴포넌트에 전달합니다.
    
5. `Section`은 자신의 자식 컴포넌트들을 `<LevelContext value={level}>`로 감쌉니다.
    
6. `Heading`은 `useContext(LevelContext)`를 통해 **가장 가까운 상위 컨텍스트 값**을 가져옵니다.
    
7. **같은 컴포넌트에서 context를 사용하며 제공하기**
    

```jsx
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Section({ children }) {
  const level = useContext(LevelContext); // 요걸로 바뀜
  return (
    <section className="section">
      <LevelContext value={level + 1}>// 요걸로 바뀜
        {children}
      </LevelContext>
    </section>
  );
}
```

번외)

- [내가 Context API 대신 Zustand 라이브러리를 사용한 이유](https://velog.io/@jsh3418/%EB%82%B4%EA%B0%80-Context-API-%EB%8C%80%EC%8B%A0-Zustand-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-%EC%9D%B4%EC%9C%A0) // → 저도 처음 앎 이런 대안도 있다고 합니다