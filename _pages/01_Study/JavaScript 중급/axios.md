---
추가 일시: Invalid date
강의: 자바스크립트로 리퀘스트 보내기
---
- [[#axios 문법]]
- [[#axios 오류 처리하기]]

  

# axios 문법

- `axios.get` : GET 리퀘스트를 보낼 때
- body 내용을 파싱할 필요 없이 데이터 프로퍼티로 가져오면 된다

```JavaScript
export async function getColorSurvey(id) {
	const res = await axios.get(`https://..../${id}`);
	//const data = res.data;
	//return data;
	return res.data;
```

```JavaScript
export async function getColorSurveys(params={}){
	const res = await axios.get(`https://....`, {
		params});
	return res.data;
}
```

- params라는 프로퍼티로 params 객체를 설정
- 쿼리 파라미터를 담고 있는 객체를 전달하면 객체에 있는 프로퍼티로 알아서 쿼리 스트링을 만들고 URL뒤에 붙여서 리퀘스트를 보내줌
- 프로퍼티 값이 null이거나 undefined인 경우에는 프로퍼티를 무시하고 URL을 만든다

```JavaScript
export async function createColorSurvey(surveyData) {
	const res = await axios.post(`https://....`,
		surveyData,
	);
	return res.data;
```

  

- axios는 **인스턴스를 생성할 수 있다**
    - 리퀘스트마다 공통되는 부분이 있으면 그걸 인스턴스에서 설정할 수 있다.

```JavaScript
const instance = axios.create({
	baseURL : 'https://....',
	timeout : 3000,
});

export async function getColorSurvey(id) {
	const res = await instance.get(
		`/color-surveys/${id}`);
	return res.data;
```

# axios 오류 처리하기

  

- axios는 400이나 500에러 리스폰스가 들어와도 Promise가 reject된다
- 200대의 상태코드를 가진 리스폰스가 돌아와야만 Promise가 fulfilled된다

  

`.response`로 확인

```JavaScript
try{
	const survey = await createColorSurvey(surveyData);
	console.log(survey)
}catch(e){
	console.log(e.response);
	console.log(e.response.status);
	console.log(e.response.data);
}
```

  

- 리스폰스가 돌아왔을 때만 리스폰스 객체가 존재한다
    
    - 리퀘스트가 성공했는지 확인하는게 좋음!
    
    ```JavaScript
    try{
    	const survey = await createColorSurvey(surveyData);
    	console.log(survey)
    }catch(e){
    	if(e.response){
    		console.log(e.response);
    	}else {
    		console.log('리퀘스트가 실패했습니다!');
    	}
    }
    ```