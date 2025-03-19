# 11. 네임스페스 패턴

네임스페이스는 코드 단위를 고유한 식별자로 그룹화한 것을 뜻한다.
전역 네임스페이스 내에 존재하는 다른 객체나 변수와의 충돌을 방지함에 있어 유용하다. 또한, 프로그램의 기능들을 체계적으로 구성하여 코드의 재사용성과 관리의 편의성을 높여준다.

JS는 다른 언어들처럼 네임스페이스를 기본적으로 지원하지는 않지만, 객체와 클로저를 활용하여 비슷한 효과를 얻을 수 있다.

## 11.2 단일 전역 변수 패턴

하나의 전역 변수를 주요 참조 객체로 사용하는 방식

```jsx
const myUniqueApplication = (() => {
	function myMethod() {
		// 코드
		return;
	}
	return {
		myMethod,
	}
})()

myUniqueApplication.myMethod();
```

단일 전역 변수 패턴은 특정 상황에서 유용할 수 있지만, 가장 큰 문제점은 다른 개발자가 같은 이름의 전역 변수를 이미 사용하고 있을 가능성이 있다는 것이다.

## 11.3 접두사 네임스페이스 패턴

피터미쇼(Peter Michaux)가 제안한 단일 전역 변수 문제에 대한 해결책중 하나이다.
먼저 고유한 접두사(ex: myApplication_)를 선정한 다음에 모든 메서드, 변수, 객체를 이 접두사 뒤에 붙여 정의한다.

```jsx
const myApplication_propertyA = {};
const myApplication_propertyB = {};
function const myApplication_myMethod() {
	// ...
}
```

전역에서 특정 변수와 이름이 겹칠 가능성을 줄인다. 하지만, 어플리케이션이 커짐에 따라 전역 객체가 생성된다는 점이다.

## 11.4 객체 리터럴 표기법 패턴

일종의 객체로, 키와 값으로 이뤄진 집합을 가지며, 각 키와 값은 콜론(:) 으로 구분된다. 키 자체가 새로운 네임스페이스가 될 수 있다.

```jsx
const myApplication = {
	getInfo() {
		//...
	},
	models: {},
	views: {
		pages: {},
	},
	collections: {},
}
```

네임스페이스에 속성을 직접 추가하는 방법 예시

```jsx
myApplication.foo = () => "bar";

myApplication.utils = {
	toString() {
		// ...
	},
	export() {
		// ...
	}
}
```


전역 네임스페이스를 오염시키지 않으면서도 코드와 매개변수를 논리적으로 구성하는데 도움을 준다. 

네임스페이스가 존재하는지 확인하고, 존재하지않는다면 새로 정의하는 몇가지 방법

```js
const myApplication = {}

var myApplication = myApplication || {}; // 1
if(!myApplication) { myApplication = {} } // 2
window.myApplication || (window.myApplication = {}) // 3
var myApplication = $.fn.myApplication = function() {} // 4
var myApplication = myApplication === undefined ? {} // 5
```

### 객체 리터럴의 장점
키-값 구조이기 때문에 손쉽게 알아볼 수 있다.
서로 다른 로직이나 기능을 쉽게 캡슐화하여 깔끔하게 분리하고, 코드 확장에 있어 든든한 기반 제공한다.

> JSON은 사실 객체 리터럴 표기법의 서브셋(subset)이고, 문법적으로 약간의 차이만 있다.


## 11.5 중첩 네임스페이스 패턴

다른 패턴에 비해 충돌위험이 낮은 편이다. 같은 이름의 네임스페이스가 존재하더라도 하뮈에 중첩된 네임스페이스까지 정확하게 일치할 가능성은 낮기 때문

```js
myApp.routers = myApp.routers || {};
myApp.model = myApp.model || {};
myApp.model.special = myApp.model.special || {};

// 필요에 따라 폭잡하게 만들 수 있다.
myApp.utilities.charting.html5.plotGraph(/**/) ;
myApp.hello.world.html5.plotGraph(/**/) 

```


다음과 같이 중첩 네임스페이스/속성을 인덱싱된 속성으로 선언할 수 있다.

```js
myApp["routers"] = myApp["routers"] || {};
myApp["models"] = myApp["models"] || {};
myApp["controllers"] = myApp["controllers"] || {};
```

두 가지 방법 모두 가독성과 구조성이 뛰어나며, 다른 프로그래밍 언어들과 유사한 네임스페이를 지원한다. 하나 주의할 점은 브라우저의 JS엔진이 먼저 myApp 객체의 위치를 찾은 후, 실제로 사용하고자하는 함수가 위치한 곳까지 파고 들어가야한다.

단일 객체 네임스페이스 패턴과 중첩 네임스페이스 패턴의 성능 차이가 크지는 않다.
- 쥬리 자이체프 개발자가 테스트 진행

## 11.6 즉시 실행 함수 표현식 패턴

즉시 실행함수는 정의 직후 바로 실행되는, 이름이 없는 함수이다. 즉시 실행 함수로 정의된 내부의 변수와 함수 모두 외부에서 접근할 수 없다. 따라서 코드의 은닉성을 구현할 수 있다.

어플리케이션의 로직을 캡슐화하여 전역 네임스페이스로부터 보호하는데 널리 사용되는 방법이다.

명확한 범위의 접근 권한(public/private 함수나 변수)과 편리한 네임스페이스 확장 기능 등을 추가할 수 있다.

```js
((namespace, undefined) => {
	// 비공개 속성들
	const foo = "foo"
	const bar = "bar"

	// 공개 속성
	namespace.foobar = "foobar"

})(window.namespace = window.namespace || {})

console.log(namespace.foobar) // foobar
```

확장성은 모든 네임스페이스 패턴에서 중요한 요소이다. 즉시실행함수 패턴을 사용하면 확장성을 비교적 쉽게 구현할 수 있다.

## 11.7 네임스페이스 주입 패턴

함수 내에서 this를 네임스페이스의 프록시로 활용하여 특정 네임스페이스에 메서드와 속성을 주입한다. 장점은 여러 객체나 네임스페이스에 기능적인 동작을 쉽게 적용할 수 있다는 것이다. 또한 이후에 확장될 기본 메서드(ex: getter/setter) 에 적용할 때 유용하다.

단점은 같은 목적을 달성하는 더 쉽고 효율적인 방법이 존재할 수 있다.

```js
const ns = ns || {};
const ns2 = ns2 || {};

const creator = function(val) {
	var val = val || 0;
	this.next = () => val++;
	this.reset = () => {
		val = 0
	}
}

creator.call(ns);
creator.call(ns2, 5000)
```

네임스페이스에 비슷한 기본 기능들을 할당할 때 유용하다. 하지만, 객체/클로저 내에서 명시적으로 기능을 선언할 때 직접접근하는 것이 불가능한 상황에서만 사용하는 것을 추천

## 11.8 고급 네임스페이스 패턴

### 11.8.1 중첩 네임스페이스 자동화 패턴

중첩 네임스페이스는 코드에 체계적이고 계층적인 구조를 만든다.

명백한 단점은 추가하고자 하는 계층이 늘어날수록 최상위 네임스페이스에 더 많은 하위 객체들이 정의되어야한다는 점이다. 특히 App이 더 복잡해져서 중첩의 깊이가 커질수록 번거로워질 수 있는 작업이다.

중첩된 네임스페이스를 자동으로 정의하는 방법이 있다. 하나의 문자열 인자를 받아 파싱한 뒤에 필요한 객체를 기반 네임스페이스에 자동으로 추가하는 간편한 방법이다.

```js
const myApp = {};

function extend(ns, ns_string) {
	const parts = ns_string.split('.');
	let parent = ns;
	let pl;

	pl = parts.length;

	for( let i=0; i<pl; i++) {
		if(typeof parent[parts[i]] === "undefined") {
			parent[parts[i]] = {};
		}
		parent = parent[parts[i]] // parent 를 갱신한다.
	}
	return parent;
}

// 사용법

const mod = extend(myApp, "module.module2");
```


### 11.8.2 의존성 선언 패턴

중첩 네임스페이스 패턴을 약간 변형한 형태인 의존성 선언 패턴이다. 객체에 대한 로컬 참조가 전체적인 조회 시간을 단축한다는 사실은 알고 있다.

```js
myApp.utilities.math.fibonacci(25);
myApp.utilities.math.sin(25);
myApp.utilities.math.plot(21,2,3);

const utils = myApp.utilities;
const math = utils.math;

// 네임스페이스에 더 쉽게 접근할 수 있다.
math.sin(25)
```

로컬 변수를 사용하는 것이 전역 변수를 매번 사용하는 것 보다 더 빠르다. 또한 후속 작업에서 중첩된 속성이나 하위 네임스페이스에 매번 접근하는 것 보다 더 편리하고 성능도 뛰어나다.

로컬 네임스페이스를 함수의 영역 상단에 선언할 것을 권장한다. 이러한 방식을 의존성 패턴이라한다.

### 11.8.3 심층 객체 확장 패턴

자동 네임스페이스 생성에 대한 또 다른 해결책은 심층 객체 확장 패턴이다. 객체 리터럴 표기법으로 선언된 네임스페이스는 다른 객체와 쉽게 확장 될 수 있다. 병합 이후에는 두 네임스페이스의 속성과 함수 모두를 동일한 네임스페이스에서 접근할 수 있다.

```js
function extendObject(dest, source) {
	for(const property in source) {
		if(source[property] && typeof source[property] === "object"
			&& !Array.isArray(source[property])
		 ) {
			 dest[property] = dest[property] || {};
			 extendObject(dest[property], source[property]);
		 } else {
		 dest[property]} = source[property]
	}
	return dest
}

const myNamespaces = myNamespaces || {};

extendObject(myNamespaces, { utils : {}, })

myNamespaces.hello.test1 = 'test1'
```

> 브라우저별 호환되는 방법이 다르므로 개념을 증명하는 정도로만 참고. 실전 프로젝트에서는 Lodash.js를 사용하는 것이 간단하고 브라우저 호환성 측면에서도 더 좋을 것임.


