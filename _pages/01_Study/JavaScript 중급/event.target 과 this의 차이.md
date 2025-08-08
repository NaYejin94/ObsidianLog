---
추가 일시: Invalid date
강의: 공부하다가 궁금했던것
---
- `event.target`: **이벤트가 실제로 발생한 요소**를 가리킴.
- `this`: **이벤트 리스너가 바인딩된 요소**를 가리킴.

  

  

const container = document.querySelector('\#container');  
container.addEventListener('click', function(event) {  
console.log('this:', this); // 이벤트 리스너가 연결된 요소 (\#container)  
console.log('event.target:', event.target); // 실제로 클릭된 요소  
});

여기서 이벤트가 실제로 발생한 요소가 \#container라면 this와 event.target은 같은 동작을 하는 거지?  
같은 동작을 한다면 this와 event.target중 어느것을 쓰는게 더 권장될까?