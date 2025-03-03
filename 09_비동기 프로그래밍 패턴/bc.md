# 9장 비동기 프로그래밍 패턴

비동기 JS프로그래밍은 프라우저가 이벤트에 응답하여 다른 코드를 실행하는 동안, 백그라운드에서 오랜 시간이 걸리는 작업을 수행하게 해줍니다.

프로미스(Promise), 비동기(Async/Await)등의 자바스크립트 개념은 메인 스레드를 차단하지 않으면서도 코드를 깔끔하고 읽기 쉽게 만들어준다.

## 9.1 비동기 프로그래밍

JS에서 동기 코드는 블로킹(blocking)방식으로 실행된다. 코드가 순서대로 한 번에 한 문장씩 실행됨을 의미한다.
반면에, 비동기 코드는 논블로킹(nonblocking)방식으로 실행된다. 즉, 자바스크립트 엔진은 현재 실행 중인 코드가 다른 작업을 기다리는 동안 백그라운드에서 해당 비동기 코드를 실행할 수 있다. 비동기 함수를 호출하면, 함수 내부의 코드가 백그라운드에서 실행되며 호출자에게 즉시 결과가 반환됩니다.

비동기코드는 일반적으로 코드를 차단하지 않고, 오래 실행되는 작업을 수행할 때, 유용하다. 네트워크 요청, 데이터베이스 읽기/쓰기, 또는 기타 I/O 작업이 적합하다.

비동기(async/await), 프로미스(promise)와 같은 자바스크립트 언어의 기능은 비동기 코드를 더 쉽게 작성할 수 있게 한다. 이러한 기법으로 코드를 마치 동기 코드처럼 작동하도록 작성할 수 있어 가독성과 이해도를 높인다.


```js
async function makeRequest(url) {
	try{
		const response = await fetch(url);
		const data = await response.json();
		console.log(data);
	} catch( error ) {
		console.error(error)
	}
}

makeRequest('test');
```

makeRequest 함수는 async 키워드로 선언되어 네트워크 요청의 결과를 기다리기 위해 await 키워드를 사용할 수 있다. 호출자는 함수 실행 중 발생할 수 있는 에러를 처리하기 위해 try-catch 키워드를 사용할 수 있다.


## 9.2 배경

콜백을 사용할 때의 주요 단점 중 하나는 ‘콜백 지옥’으로 불리는 상황을 초래할 수 있음


## 9.3 프로미스 패턴

프로미스는 비동기 작업의 결과를 나타내는 객체.
대기(pending), 완료(fulfilled), 거부(rejected)의 세 가지 상태를 가질 수 있다.

프로미스는 Promise 생성자를 사용하여 만들 수 있고, 함수를 인수로 받는다.

이 함수는 resolve, reject 두 개의 인수를 전달 받는다. resolve 함수는 비동기 작업이 성공적으로 완료됐을 때, 호출되고, reject함수는 작업이 실패했을 때 호출된다.

### 9.3.1 프로미스 체이닝

프로미스 체이닝 패턴을 사용하면 여러 개의 프로미스를 함께 연결하여 보다 복잡한 비동기 로직을 만들 수 있다.

### 9.3.3 프로미스 병렬 처리

프로미스 병렬 처리 패턴은 Promise.all 메서드를 사용하여 여러 프로미스를 동시에 실행할 수 있게 한다.

```js
Promise.all([
	makeRequest('test1')
	makeRequest('test2')
]).then(([data1, data2]) => {
	console.log(data1, data2)
})
```

### 9.3.4 프로미스 순차 실행

프로미스 순차 실행 패턴은 Promise.resolve 메서드를 사용하여 프로미스를 순차적으로 실행할 수 있도록 해준다.

```js
Promise.resolve()
	.then(() => makeRequest1())
	.then(() => makeRequest2())
	.then(() => makeRequest2())
	.then(() => {
		// 모든 요청 완료
	})
```

### 9.3.5 프로미스 메모이제이션

프로미스 메모이제이션(Promise Memoization) 패턴은 캐시를 사용하여 프로미스 함수 호출의 결과 값을 저장한다. 이를 통해 중복 요청을 방지할 수 있다.

```js
const cache = new Map();

function memoizedMakeRequest(url) {
	if(cache.has(url)) {
		return cache.get(url)
	}


	return new Promise(() => {
		// 비동기 요청 코드
	}
}
```


### 9.3.6 프로미스 파이프라인

프로미스 파이프라인 패턴은 프로미스와 함수형 프로그래밍 기법을 활용하여 비동기 처리의 파이프 라인을 생성한다. tanstack-query useQuery의 select 기능이라고 생각하면 된다.

### 9.3.7 프로미스 재시도

프로미스 재시도 패턴을 사용하면 프로미스가 실패할 때 다시 시도할 수 있다.
exponential 재시도 기법도 존재한다.

### 9.3.8 프로미스 데코레이터

프로미스 데코레이터 패턴은 고차 함수를 사용하여 프로미스에 적용할 수 있는 데코레이터를 생성한다. 프로미스에 추가적인 기능을 부여할 수 있다.

```js
function logger(fn) {
	return function(...args) {
		console.log('start');
		return fn(...args).then(result => {
			console.log('Function completed.');
			return result;
		})
	};
}
```

### 9.3.9 프로미스 경쟁

프로미스 경쟁(Promise Race) 패턴은 여러 프로미스를 동시에 실행하고 가장 먼저 완료되는 프로미스의 결과를 반환한다.
```js
Promise.race([
	makeRequest('test1')
	makeRequest('test2')
]).then(([data1, data2]) => {
	console.log(data1, data2)
})
```

## 9.4 async/await 패턴

비동기 코드를 마치 동기식 코드처럼 작성할 수 있게 해주는 자바스크립트의 기능. 프로미스를 기반으로 구축 되어있다.

try-catch 키워드를 사용하며, 실패하면 에러가 catch 블록에서 처리되고, 에러를 처리할 수 있다.

### 9.4.1 비동기 함수 조합

비동기 함수 조합(async Function Composition)패턴은 여러 비동기 함수를 조합하여 보다 복잡한 비동기 로직을 구성하는 것을 의미한다.


```js
async function makeRequest(url) {
	const response = await fetch(url);
	const data = await response.json();
	return data;
}

async function processData(data) {
	let processedData;
	// 데이터 처리 로직 생략
	return processedData;
}

async function main() {
	const data = await makeRequest('test');
	const processedData = await processData(data); // 비동기 함수를 조합한다.
	console.log(processedData)
}
```

### 9.4.2 비동기 반복

비동기 반복(aync Iteration) 패턴은 **for-await-of** 반복문을 사용하여 비동기 반복 가능 객체를 순회할 수 있도록 한다.

promise.all 패턴을 대체할 수 있다.

```js
async function* createAsyncIterable() {
	yield 1;
	yield 2;
	yield 3;
}

async function main() {
	for await (const value of createAsyncIterable()) {
		console.log(value);
	}
}
```


### 9.4.3 비동기 에러 처리

비동기 에러 처리 패턴은 try-catch 블록을 사용하여 비동기 함수 실행 도중 발생할 수 있는 에러를 처리한다.

### 9.4.5 비동기 순차 실행

Promise.resolve 메서드를 사용하여 비동기 작업을 순차적으로 실행할 수 있도록 한다.
