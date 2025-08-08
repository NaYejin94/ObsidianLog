---
추가 일시: Invalid date
강의: css 레이아웃
---
- [[#Grid 란?]]
- [[#grid 나누기]]
- [[#유연한 크기와 유용한 함수들]]
    - [[#fr]]
    - [[#minmax]]
    - [[#repeat]]
- [[#간격 넣기]]
    - [[#gap]]
- [[#크기 미리 정해두기]]
    - [[#grid-auto-rows(columns)]]
- [[#원하는 위치에 요소 배치하기]]
- [[#이름으로 배치하기]]
    - [[#grid-area]]

# Grid 란?

2차원으로 배열할 수 있는 방법

grid line

칸 → grid cell

  

격자 나누기 : grid-template-rows(columns)

간격 : gap

크기 미리 정하기 : grid-auto-rows(columns)

원하는 위치에, 여러 칸에 걸쳐서 배치 : grod-row(columns), span

이름으로 배치 : grid-area, grid-template-areas

  

# grid 나누기

  

```JavaScript
grid-template-row : 240px 240px 240px;
grid-template-columns : 240px 240px 240px;
-> 3x3 그리드

grid-template : 240px 240px 240px / 240px 240px 240px;
-> 가로 세로를 한꺼번에 쓸수도 있다.(가로 세로 순)
```

# 유연한 크기와 유용한 함수들

### fr

: 크기를 유연하게 지정

```JavaScript
grid-template-row : 1fr 1fr 2fr;
```

  

### minmax

: 너비의 최소, 최대 값을 지정해줌 (최대 값에만 fr를 줄 수 있음)

  

### repeat

```JavaScript
repeat(6,1fr) -> 그리드를 똑같은 비율로 6개 나눔
```

# 간격 넣기

### gap

# 크기 미리 정해두기

### grid-auto-rows(columns)

# 원하는 위치에 요소 배치하기

```JavaScript
grid-row:3
grid-column:5 -> 3행 5열에 배치

grid-row:3/5
grid-column :3 -> 3행부터 5행 / 3열에 배치

grid-row:3/span 2;
grid-column:2/span3: -> 3행부터 2칸 / 2열부터 3칸 차지
```

# 이름으로 배치하기

### grid-area

grid-template-areas

```JavaScript
grid-template-areas:
"r g
r b"
# 비워두고 싶을 때는 .
```