# 09. 비동기 프로그래밍 패턴

프로미스, Async/await 등의 자바스크립트 개념은 메인 스레드를 차단하지 않으면서도 코드를 깔끔하고 읽기 쉽게 만들어준다. 이러한 기능을 활용해 애플리케이션 흐름을 효율적으로 구성하는 몇 가지 패턴에 대해 설명한다.

## 9.1 비동기 프로그래밍

비동기 코드는 논블로킹 방식으로 실행된다. 일반적으로 코드의 나머지 부분을 차단하지 않고 오래 실행되는 작업을 수행할 때 유용하다. 네트워크 요청, 데이터베이스 읽기/쓰기, 또는 기타 입출력 작업이 비동기 코드를 사용하기 적합하다.

## 9.2 배경

콜백 함수는 다른 함수에 인수로서 전달되어, 비동기 작업이 완료된 후 실행된다. 콜백을 사용할 때의 주요 단점은 중첩된 콜백 구조로 인해 코드의 가독성과 유지보수성이 크게 저하되는 '콜백 지옥'으로 불리는 상황을 초래할 수 있다는 것이다.

## 9.3 프로매스 패턴

프로미스는 비동기 작업을 처리하는 방법으로, 비동기 작업의 결과를 나타내는 대기(pending), 완료(fulfilled), 거부(rejected)의 세 가지 상태를 가질 수 있다.

프로미스를 사용하면 콜백보다 체계적이고 가독성이 높은 방법으로 비동기 작업을 처리할 수 있다.

### 9.3.1 프로미스 체이닝

프로미스 체이닝 패턴을 사용하면 여러 개의 프로미스를 함께 연결하여 보다 복잡한 비동기 로직을 만들 수 있다.

### 9.3.2 프로미스 에러 처리

프로미스 에러 처리 패턴은 catch 메서드를 사용하여 프로미스 체인의 실행 중에 발생할 수 있는 에러를 처리한다.

```javascript
promiseTest()
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

### 9.3.3 프로미스 병렬 처리

프로미스 병렬 처리 패턴은 Promise.all 메서드를 사용하여 여러 프로미스를 동시에 실핼할 수 있게 해준다.

```javascript
Promise.all([promiseTest(), promiseTest1()]).then(([data1, data2]) => {
  console.log(data1, data2);
});
```

### 9.3.4 프로미스 순차 실행

프로미스 순차 실행 패턴은 Promise.resolve 메서드를 사용하여 프로미스를 순차적으로 실행할 수 있게 해준다.

```javascript
Promise.resolve()
  .then(() => promiseTest())
  .then(() => promiseTest1())
  .then(() => promiseTest2());
```

### 9.3.5 프로미스 메모이제이션

프로미스 메모이제이션 패턴은 캐시를 사용하여 프로미슿 ㅏㅁ수 호출의 결과값을 저장하고 이를 통해 중복된 요청을 방지할 수 있다.

```javascript
const cache = new Map();

const memoized = (param) => {
  if (cache.has(param)) {
    return cache.get(param);
  }

  return new Promise((resolve, reject) => {
    fetch(param)
      .then((response) => response.json())
      .then((data) => {
        cache.set(param, data);
        resolve(data);
      })
      .catch((error) => reject(error));
  });
};
```

### 9.3.6 프로미스 파이프라인

프로미스 파이프라인 패턴은 프로미스와 함수형 프로그래밍 기법을 활용하여 비동기 처리의 파이프 라인을 생성한다.

### 9.3.7 프로미스 재시도

프로미스 재시도 패턴을 사용하면 프로미스가 실패할 때 다시 시도할 수 있다.

### 9.3.8 프로미스 데코레이터

프로미스 데코리에터 패턴은 고차 함수를 사용하여 프로미스에 적용할 수 있는 데코레이터를 생성하고 이를 통해 프로미스에 추가적인 기능을 부여할 수 있다.

### 9.3.9 프로미스 경쟁

프로미스 경쟁 패턴은 여러 프로미스를 동시에 실행하고 가장 먼저 완료되는 프로미스의 결과를 반환한다.

```javascript
Promise.race([promiseTest(), promiseTest1()]).then((data) => {
  console.log(data);
});
```

## 9.4 async/await 패턴

async/await은 프로미스를 기반으로 구축되었으며, 비동기 코드를 마치 동기 코드처럼 작성할 수 있게 한다.

### 9.4.1 비동기 함수 조합

비동기 함수 조합 패턴은 여러 비동기 함수를 조합하여 보다 복잡한 비동기 로직을 구성하는 것을 의미한다.

### 9.4.2 비동기 반복

비동기 반복 패턴은 for-await-of 반복문을 사용하여 비동기 반복 가능 객체를 순회할 수 있게 한다.

### 9.4.3 비동기 에러 처리

비동기 에러 처리 패턴은 try- catch 블록을 사용하여 에러를 처리한다.

```javascript
async function main() {
  try {
    const data = await test();
  } catch (error) {
    console.log();
  }
}
```

### 9.4.4 비동기 병렬

비동기 병렬 패턴은 Promise.all 메서드를 사용하여 여러 비동기 작업을 동시에 실행할 수 있게 한다.

```javascript
async function main() {
  const [data1, data2] = await Promise.all([test1(), test2()]);
}
```

### 9.4.5 비동기 순차 실행

비동기 순차 실행 패턴은 Promise.resolve 메서드를 사용하여 비동기 작업을 순차적으로 실행할 수 있게 한다.

```javascript
async function main() {
  let result = await Promise.resolve();
  result = await test1(result);
  result = await test2(result);
}
```

### 9.4.6 비동기 메모이제이션

비동기 메모이제이션 패턴은 캐시를 사용해 비동기 함수 호출 결과를 저장하여 중복 요청을 방지한다.

```javascript
const cache new map();

const memoized = (param) => {
  if (cache.has(param)) {
    return cache.get(param);
  }
const response = await fetch(param);
const data = await response.json();
cache set(param, data);
return data;
};

```

### 9.4.7 비동기 이벤트 처리

비동기 이벤트 처리 패턴은 비동기 함수를 사용하여 이벤트를 처리할 수 있게 한다.

### 9.4.8 async/await 파이프라인

async/await 파이프라인 패턴은 async/await와 함수형 프로그래밍 기법을 활용하여 비동기 변환 작업들의 파이프라인을 생성한다.

### 9.4.9 비동기 재시도

비동기 재시도 패턴을 사용하면 비동기 작업이 실패해도 자동으로 재시도할 수 있게 한다.

### 9.4.10 async/await 데코레이터

async/await 데코레이터 패턴은 고차 함수를 사용하여 데코레이터를 생성하고, 생성된 데코레이터는 비동기 함수에 적용되어 추가적인 기능을 제공한다.

```javascript
const asyncLogger(fn){
    return async function(...args){
        return await fn(...args)
    }
}
@asyncLogger
async function main(){
   const result =  await test();
}
```
