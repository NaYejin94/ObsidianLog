---
추가 일시: Invalid date
강의: css 레이아웃
---
- [[#flexbox란]]
- [[#배치 방향 : flex-direction]]
- [[#정렬]]
- [[#요소가 넘칠 때]]
- [[#간격]]
    - [[#gap]]
- [[#요소 꽉 채우기]]
    - [[#flex-grow]]
    - [[#flex-shrink]]

# flexbox란

flexbox에서 요소를 배치하는 방법

→ 배치할 방향 : flex-direction

→ 정렬하기 : justify-content, align-items

→ 요소가 넘칠 때 : flex-wrap

→ 요소 간격 : gap

→ 크기 늘이거나 줄이기 : flex-grow, flex-shrink, flex-basis

  

```JavaScript
disply : flex;
```

# 배치 방향 : flex-direction

```JavaScript
flex-direction : row/column (세로/가로)

floex-direction : row-reverse / column-reverse;
```

  

# 정렬

→ 요소가 배치되는 방향 : 기본 축(main axis)

→ 기본 축에 수직인 방향 : 교차 축(cross axis)

→ justify-content : 기본 축 정렬

: flex-end

:flex-start

: space-around

: space-between

→ align-items : 교차 축 정렬

: stretch

# 요소가 넘칠 때

```JavaScript
flex-wrap : wrap
-> 교차 축 방향으로 넘어가서 배치됨 
```

# 간격

### gap

```JavaScript
gap : 30px;
gap : 30px 60px;
```

# 요소 꽉 채우기

### flex-grow

: 요소를 늘려서 빈 공간을 꽉 채우고 싶을 때

```JavaScript
flex-grow : 1; -> 상대적인 값, 숫자에 비례해서 더 많이 늘어남
```

### flex-shrink

: 값이 클수록 더 많이 줄어 듬

: 요소의 크기를 고정하고 싶을 때 → flex-shrink: 0 ;