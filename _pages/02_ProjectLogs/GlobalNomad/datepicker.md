# 🛠 프로젝트 이름: GlobalNomad

- 🗓 시작일: 2025-05-26
- 📅 오늘 날짜: 2025-05-25
- 👥 팀원: 
- 🔗 GitHub / 배포 링크: 

---

##### datepicker 구현

1. React DayPicker, date-fns 설치
```jsx
npm install react-day-picker date-fns
```

**react-day-picker**
- <mark style="background: #ADCCFFA6;">날짜 선택 UI 컴포넌트를 제공하는 React 라이브러리</mark>
- 특징
	- 가볍고 빠름
	- 높은 커스터마이징
	- Headless 구성 가능
- 언제 쓰면 좋을까?
	- 공통적인 날짜 선택 UI가 필요한 경우
	- 단일/범위/다중 선택 등 다양한 옵션을 유연하게 처리하고 싶을 때
	- 커스터마이징 가능한 달력을 만들고 싶을 때


**date-fns**
- <mark style="background: #ADCCFFA6;">날짜를 계산, 조작, 포맷팅하는 JS 유틸리티 라이브러리</mark>
- 특징
	- 모듈별 개별 import -> 필요한 기능만 가져와서 번들 사이즈 작음
	- 함수형 스타일 -> 조작 함수가 순수 함수라 테스트 및 예측 가능
	- TypeScript 완벽 지원
- 언제 쓰면 좋을까?
	- 날짜 포맷팅이 필요할 때 (yyyy-MM-dd, MM/dd/yyyy 등)
	- 날짜 간의 차이, 비교, 계산 등 로직을 쉽게 처리하고 싶을 때


**react-day-picker + date-fns 조합의 장점**
- react-day-picker는 날짜 UI를 담당하고
- date-fns는 날짜 로직을 담당한다.
- 둘을 조합하면 <mark style="background: #ADCCFFA6;">날짜 선택 후 보여주는 형식을 자유롭게 다룰 수 있어</mark>서 유연하고 확장성이 좋다.


---


