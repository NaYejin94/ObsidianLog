🐞 버그 설명  
발생 페이지: (예: Items Page)  
설명: 모달을 띄울때 콘솔에 찍힌 오류  
React does not recognize the showWarning prop on a DOM element. If you intentionally want it to appear in the DOM as a custom attribute, spell it as lowercase showwarning instead. If you accidentally passed it from a parent component, remove it from the DOM element.

원인 : 해당 prop이 스타일 컴포넌트를 통해 생성된 DOM 요소에 직접 전달되었기 때문에.  
React는 기본 HTML 요소에서 인식할 수 없는 props를 자동으로 필터링 하지 않기 때문에 오류 발생

1. attrs를 사용해서 필터링

```
const DonatioVisibleDiv = styled.div.attrs((props) => ({
  style: { display: props.showWarning ? "block" : "none" },
}))`
  height: 14px;
  font-size: 12px;
  color: #ff2626;
  margin-top: 7px;
`;
```

2. as 속성으로 변경 : as 속성을 사용하면 원하지 않는 prop이 DOM에 전달되지 않도록 방지

```
<DonatioVisibleDiv as="div" showWarning={showWarning}>
  갖고 있는 크레딧보다 더 많이 후원할 수 없어요
</DonatioVisibleDiv>
```

3. prop을 필터링해서 전달 : porp에 $를 붙이면 스타일 컴포넌트가 이를 DOM요소에 전달하지 않고 내부적으로만 사용
