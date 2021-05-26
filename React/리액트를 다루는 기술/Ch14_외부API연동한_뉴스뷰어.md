# 14.1 비동기 작업의 이해

웹 애플리케이션에서 **시간이 걸리는 작업**       

#### ex1) 웹 애플리케이션에서 **서버 쪽 데이터**가 필요할 때 - Ajax 기법을 사용해 **서버의 API 호출**      
**서버의 API 사용시** 네트워크 송수신 과정에서 시간이 걸림      
응답을 기다리고 전달받은 응답 데이터 처리함 ⇒ **비동기적으로 처리**     

비동기적으로 처리는 동시에 여러가지 요청을 처리 가능하며, 기다리는 과정에서 다른 함수 호출도 가능

### 동기적 vs. 비동기적

동기적(synchronous) | 비동기적(asynchronous)
-----------------|------------------|
요청이 끝날 때까지 중지 상태 → 다른 작업 불가 | 동시에 여러가지 요청 가능, 기다리면서 다른 함수 호출 가능

#### ex2) **`setTimeout` 함수로 특정 작업을 예약**할 때       
`setTimeout` 함수: 지정된 시간이 흐른 후 함수 실행 
```jsx
funtion printMe() {
	console.log('Hello World!');
}
setTimeout(printMe, 3000);
console.log('대기 중...');
```
⇒ 실행결과: 코드가 위부터 아래까지 다 호출되고 3초 뒤에 지정한 printMe 함수가 호출   
여기서 **`printMe`** 함수는 **`콜백함수`**!!

### 콜백 함수

**JavaScript**에서 **비동기 작업**을 하는 가장 흔한 방법     


### `Promise`

**콜백 지옥 코드(중첩)가 형성되지 않도록** 하는 방안     
`.then`을 사용해 **연속된 작업 처리**하므로 중첩이 형성되지 않음


### `async`/`await`

**`Promise`를 더 쉽게 사용**할 수 있도록 함     
**`함수의 앞 부분`에 `async` 키워드 추가**하고, 해당 함수 내부에서 **`Promise 앞부분`에 `await` 키워드** 사용     
→ Promise가 끝날 때까지 기다리고, 결과 값을 특정 변수에 담을 수 있음


# 14.2 axios로 API 호출해서 데이터 받아오기

### axios

- **자바스크립트 HTTP 클라이언트**
- 현재 가장 많이 사용
- HTTP 요청을 `Promise` 기반으로 처리

**`axios.get('주소').then(결과응답)`**   
→ (**비동기 처리**) 파라미터로 전달된 주소에 GET 요청을 한 후, `.then`으로 응답(결과) 확인!!   
```jsx
const onClick = () => {
	axios.get('주소').then(response => { 
		setData(response.data);
	});
};
```

async/await 적용시 **`async () => {}`** 형식
```jsx
const onClick = async () => {
	try {
		const response = await axios.get('주소');
		setData(response.data);
	} catch(e) {
		console.log(e);
	}
};
```


