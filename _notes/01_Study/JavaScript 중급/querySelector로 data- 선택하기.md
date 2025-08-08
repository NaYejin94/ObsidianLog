---
추가 일시: Invalid date
강의: 공부하다가 궁금했던것
---
```JavaScript
<span class="word" data-word="Codeit" style="">Codeit</span>

const word = document.querySelector(`[data-word="${input.value}"]`);
```

- querySelector : css선택자 방법으로 요소를 선택하기
- `` `[data-word="${input.value}"]` `` : span태그의 data-word 속성 중 input 창에 입력한 값 {input.value}와 일치하는 요소 선택

  

- HTML 태그의 비표준 속성을 활용할 때 HTML태그에 `data-*`형태로 속성 작성
- dataset 프로퍼티를 활용하면 안전하게 비표준 속성을 다룰 수 있다

```JavaScript
<div class="room-1" data-title="침실">

if(e.target.dataset.title){
	...
}
```