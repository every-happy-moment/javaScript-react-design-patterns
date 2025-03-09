# 09_비동기 프로그래밍 패턴
비동기 자바스크립트 프로그래밍은 브라우저가 이벤트에 응답하여 다른 코드를 실행하는 동안, 백그라운드에서 오랜 시간이 걸리는 작업을 수행하게 해준다. 프로미스(promise), 비동기(async/await) 등의 자바스크립트 개념은 메인 스레드를 차단하지 않으면서도 코드를 깔끔하고 읽기 쉽게 만들어준다.

## 9.1 비동기 프로그래밍
비동기 코드는 논블로킹(nonblocking) 방식으로 실행된다. 즉, 자바스크립트 엔진은 현재 실행 중인 코드가 다른 작업을 기다리는 동안 백그라운드에서 해당 비동기 코드를 실행할 수 있다. 비동기 함수를 호출하면, 함수 내부의 코드가 백그라운드에서 실행되며 호출자에게 즉시 결과가 반환된다.<br />
비동기 코드는 일반적으로 코드의 나머지 부분을 차단하지 않고 오래 실행되는 작업을 수행할 때 유용하다. 네트워크 요청, 데이터베이스 읽기/쓰기, 또는 기타 I/O(입력/출력) 작업이 비동기 코드를 사용하기 적합하다.

## 9.2 배경
자바스크립트에서 콜백 함수는 다른 함수에 인수로 전달되어, 비동기 작업이 완료된 후 실행된다. 콜백 함수는 주로 네트워크 요청이나 사용자 입력과 같은 비동기 작업의 결과를 처리하기 위해 사용되었다. 콜백을 사용할 때의 주요 단점 중 하나는 '콜백 지옥'으로 불리는 상황을 초래할 수 있다는 것이다. 콜백 지옥은 중첩된 콜백 구조르 인해 코드의 가독성과 유지보수성이 크게 저하되는 상황을 뜻한다.

## 9.3 프로미스 패턴
프로미스는 비동기 작업의 결과를 나타내는 객체로, 대기(pending), 완료(fulfilled), 거부(rejected)의 세 가지 상태를 가질 수 있다. <br />
프로미스는 Promise 생성자를 사용하여 만들 수 있으며, 이 생성자는 함수를 인수로 받는다. 또다시 이 함수는 resolve와 reject 두 개의 인수를 전달받는다. resolve 함수는 비동기 작업이 성공적으로 완료되었을 때 호출되고, reject 함수는 작업이 실패했을 때 호출된다.<br />
프로미스를 사용할 때의 주요 장점 중 하나는 콜백보다 체계적이고 가독성이 높은 방법으로 비동기 작업을 처리할 수 있다는 것이다. 이를 통해 '콜백 지옥'을 피하고, 이해하기 쉽고 유지보수성이 높은 코드를 작성할 수 있다.

### 9.3.1 프로미스 체이닝
프로미스 체이닝 패턴을 사용하면 여러 개의 프로미스를 함께 연결하여 보다 복잡한 비동기 로직을 만들 수 있다.

### 9.3.2 프로미스 에러 처리
프로미스 에러 처리 패턴은 catch 메서드를 사용하여 프로미스 체인의 실행 중에 발생할 수 있는 에러를 처리한다.

### 9.3.3 프로미스 병렬 처리
프로미스 병렬 처리 패턴은 Promise.all 메서드를 사용하여 여러 프로미스를 동시에 실행할 수 있게 해준다.

### 9.3.4 프로미스 순차 실행
프로미스 순차 실행 패턴은 Promise.resolve 메서드를 사용하여 프로미스를 순차적으로 실행하 수 있도록 해준다.
```javascript
Promise.resolve()
.then(() => makeRequest1())
.then(() => makeRequest2())
.then(() => makeRequest3())
.then(() => {
    // 모든 요청 완료
});
```

### 9.3.5 프로미스 메모이제이션
프로미스 메모이제이션 패턴은 캐시를 사용하여 프로미스 함수 호출의 결과값을 저장한다. 이를 통해 중복된 요청을 방지할 수 있다.

### 9.3.6 프로미스 파이프라인
프로미스 파이프라인 패턴은 프로미스와 함수형 프로그래밍 기법을 활용하여 비동기 처리의 파이프라인을 생성한다.

### 9.3.7 프로미스 재시도
프로미스 재시도 패턴을 사용하면 프로미스가 실패할 때 다시 시도할 수 있다.

### 9.3.8 프로미스 데코레이터
프로미스 데코레이터 패턴은 고차 함수를 사용하여 프로미스에 적용할 수 있는 데코레이터를 생성한다. 이를 통해 프로미스에 추가적인 기능을 부여할 수 있다.

### 9.3.9 프로미스 경쟁
프로미스 경쟁 패턴은 여러 프로미스를 동시에 실행하여 가장 먼저 완료되는 프로미스의 결과를 반환한다.
```javascript
Promise.race([
    makeRequest('http://example.com/1'),
    makeRequest('http://example.com/2')
]).then(data => {
    console.log(data);
});
```

## 9.4 async/await 패턴
async/await는 비동기 코드를 마치 동기 코드처럼 작성할 수 있게 해주는 자바스크립트의 기능이다. async/await는 프로미스를 기반으로 구축되었으며, 비동기 코드 작업을 보다 쉽고 간결하게 만들어준다.<br />
함수 내부에서 await 키워드는 fetch 호출이 완료될 때까지 함수의 실행을 일시 중지한다. 요청이 성공하면 데이터가 콘솔에 기록되고, 실패하면 에러가 catch 블록에서 처리되어 콘솔에 기록된다.

## 9.4.1 비동기 합수 조합
비동기 함수 조합 패턴은 여러 비동기 함수를 조합하여 보다 복잡한 비동기 로직을 구성하는 것을 의미한다.
```javascript
async function main(){
    const data = await makeRequest('http://example.com/');
    const processedData = await processData(data);
    console.log(processedData);
}
```

### 9.4.2 비동기 반복
비동기 반복 패턴은 for-await-of 반복문으르 사용하여 비동기 반복 가능 객체를 순회할 수 있도록 한다.
```javascript
async function* createAsyncIterable(){
    yield 1;
    yield 2;
    yield 3;
}

async function main(){
    for await (const value of createAsyncIterable()){
        console.log(value);
    }
}
```

### 9.4.3 비동기 에러 처리
비동기 에러 처리 패턴은 try-catch 블록을 사용하여 비동기 함수 실행 도중 발생하 ㄹ수 있는 에러를 처리한다.

### 9.4.4 비동기 병렬
비동기 병렬 패턴은 Promise.all 메서드를 사용하여 여러 비동기 작업을 동시에 실행할 수 있게 한다.

### 9.4.5 비동기 순차 실행
비동기 순차 실행 패턴은 Promise.resolve 메서드를 사용하여 비동기 작업을 순차적으로 실행할 수 있도록 한다.

### 9.4.6 비동기 메모이제이션
비동기 메모이제이션 패턴은 캐시를 사용해 비동기 함수 호출 결과를 저장하여 중복 요청을 방지할 수 있다.

### 9.4.7 비동기 이벤트 처리
비동기 이벤트 처리 패턴은 비동기 함수를 사용하여 이벤트를 처리할 수 있도록 한다.

### 9.4.8 async/await 파이프라인
async/await 파이프라인 패턴은 async/await 와 함수형 프로그래밍 기법을 활용하여 비동기 변환 작업들의 파이프라인을 생성한다.

### 9.4.9 비동기 재시도
비동기 재시도 패턴을 사용하면 비동기 작업이 실패해도 자동으로 재시도할 수 있다.

### 9.4.10 async/await 데코레이터
async/await 데코레이터 패턴은 고차 함수를 사용하여 데코레이터(decorator)를 생성한다. 이 데코레이터는 비동기 함수에 적용되어 추가적인 기능을 제공한다.

## 9.5 실용적인 예제 더보기
### 9.5.1 HTTP 요청보내기
### 9.5.2 파일 시스템에서 파일 읽어오기
### 9.5.3 파일 시스템에 파일 쓰기
### 9.5.4 여러 비동기 함수를 한 번에 실행하기
### 9.5.5 여러 비동기 함수를 순서대로 실행하기
### 9.5.6 함수의 결과를 캐싱하기
```javascript
const cache = new Map();

async function makeRequest(url){
    if(cache.has(url)){
        return cache.get(url);
    }

    try{
        const response = await fetch(url);
        const data = await response.json();
        cache.set(url, data);
        return data;
    }catch(error){
        throw error;
    }
}
```

### 9.5.7 async/await 로 이벤트 처리하기
### 9.5.8 비동기 함수 실패 시 자동으로 재시도하기
```javascript
async function makeRequest(url){
    try{
        const response = await fetch(url);
        const data = await response.json();
        return data;
    }catch(error){
        throw error;
    }
}

async function retry(fn, maxRetries = 3, retryDelay = 1000){
    let retries = 0; 
    while(retries <= maxRetries){
        try{
            return await fn();
        }catch(error){
            retries++;
            console.error(error);
            await new Promise((resolve) => setTimeout(resolve, retryDelay));
        }
    }
    throw new Error(`Failed after ${retries} retries.`);
}

retry(() => makeRequest("http://example.com/")).then((data) => {
    console.log(data);
})
```

### 9.5.9 async/await 데코레이터 작성하기
```javascript
function asyncDecorator(fn){
    return async function (...args){
        try{
            return await fn(...args);
        }catch(error){
            throw error;
        }
    };
}

const makeRequest = asyncDecorator(async function(url){
    const response = await fetch(url);
    const data = await response.json();
    return data;
});

makeRequest("http://example.com/").then((data) => {
    console.log(data);
});
```

