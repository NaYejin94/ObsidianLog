---
추가 일시: Invalid date
강의: React 웹 개발 시작하기
---
- [[#리액트 프로젝트]]

# 리액트 프로젝트

```JavaScript
npm init react-app .
```

- create-react-app 으로 리액트 프로젝트 생성
    
    `npm init react-app <폴더이름>`
    
- 개발 모드 실행
    
    `npm run start`
    
- 개발 모드 종료
    
    Ctrl + C
    
      
    

→ 현재는 create react app은 더이상 관리되지 않는 프로젝트 이기 때문에

→ ==**프로젝트 생성에 Vite라는 도구를 쓰는게 권장**==

  

- 프로젝트 생성하기

```JavaScript
npm create vite@latest . -- --template react
```

  

- 필요한 패키지 설치

```JavaScript
npm install
```

  

- 프로젝트 실행

```JavaScript
npm run dev
```