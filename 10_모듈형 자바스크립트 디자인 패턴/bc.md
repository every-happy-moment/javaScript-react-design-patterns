# 10장 모듈형 자바스크립트 디자인 패턴

확장가능한 js환경에서 모듈형(module)이란 서로 의존성이 낮은 기능들이 모듈로써 저장된 형태를 뜻한다. 이러한 느슨한 결합은 의존성을 제거하여 애플리케이션의 유지보수성을 용이하게 만들어준다.

## 10.1 스크립트 로더에 대한 참고사항

AMD와 CommonJS 같은 모듈형 자바스크립트를 이해하기 위해서는 스크립트 로더에 대한 설명이 필수적이다. 스크립트 로더는 모듈형 자바스크립트를 구현하기 위한 핵심적인 도구.

AMD와 CommonJS 형식의 모듈로딩을 지원하는 로더중 RequireJS와 curl.js가 있다.

## 10.2 AMD

AMD(Asynchronous Module Definition) 모듈 형식은 모듈과 의존성 모두를 비동기적으로 로드할 ㅅ ㅜ있도록 설계된 모듈 정의 방식이다. 주요 목표는 개발자들이 활용할 수 있는 모듈형 자바스크립트 솔루션을 제공하는 것이다.
- 비동기적이면서 높은 유연성을 갖음

CommonJS AMD 형식 이라는 용어가 가끔 사용되기도 하지만, CommonJS 진영의 모두가 AMD 형식을 지향하는 것은 아니기 때문에, AMD 또는 비동기 모듈 지원이라고 부르는 것이 적절

### 10.2.1 모듈 알아보기

AMD에서 주목할만한 가장 중요한 두 가지 개념은 모듈 정의를 구현하는 define 메서드와 의존성 로딩을 처리하는 require 메서드로 나뉜다. 

```js
define(
	module_id?
	[dependencies?]
	definition function {} /* 모듈이나 객체를 인스턴스화하는 함수 */
)
```

module_id 인자를 생략하면 **익명모듈** 이라 한다. 모듈의 아이덴티티는 DRY(Dont Repeat Yourself)원칙에 따라 파일명과 코드의 중복을 최소화  수 있다.

dependencies 인자는 모듈에서 필요로 하는 의존성 배열을 나타낸다. definition function 은 모듈을 초기화하기 위해 실행되는 함수이다.

> 예시에서 의존성을 로드하기 위해 css! 가 포함되었지만, 이 접근 방식은 css가 완전히 로드되는 시점을 확인할수 없다는 등 몇가지 주의해야할 점이 있다. 또한 빌드 과정에 따라 CSS가 최종 파일에 의존성으로 포함될 수 있기 때문에 특별한 경우를 제외하고는 의존성을 가져올때 주의해야한다.


### 10.2.2 AMD 모듈과 jQuery

jQuery는 단 하나의 파일로 제공된다. 하지만, jQuery 라이브러리의 플러그인 기반 특성을 고려할때, jQuery를 사용하는 AMD 모듈을 정의하는 것이 얼마나 쉬운지 알 수 있다.

```js
define(["jquery", "jquery.color", "lodash"], function($, colorPluginm _) {
	// ...
})
```


#### jQuery를 비동기 호환 모듈로 등록하기

AMD를 사용하면서 jQuery의 버전이 전역 공간에 노출되지 않도록 하기 위해서는 최상위 모듈 내에 noConflict 를 호출해야한다. 또한 한 페이지에서 여러버전의 jQuery를 사용하는 것이 가능하기 때문에 특별한 고려 사항을 적용해야한다. 문제들을 인식하고 있는 로더 즉 define.amd.jquery 를 포함하는 로더에만 AMD 로더로 등록한다.

```js
var jQuery = this.jQuery || "jQuery ",
$ = this.$ || "$",
originaljQuery = jQuery,
original$ = $;

define(["jquery"], ($) => {
	//...
})
```


#### 어째서 AMD 가 모듈형 자바스크립트 작성에 더 좋을까

- 유연한 모듈 정의 방식에 대한 명확한 제안 제공
- 기존에 많이 사용되고 있는 전역 네임스페이스나 `<script>` 태그 방식에 비해 훨씬 더 구조화되어있다.
- 모듈 정의가 독립적으로 유지
- CommonJS에 비해 더 효과적. AMD는 다른 크로스도메인, 로컬환경, 디버깅 등에서 문제가 없으며, 서버사이드 툴을 사용할 필요도 없다. **대부분의 AMD 로더는 빌드과정없이 브라우저에서 모듈 로딩을 지원**
- 여러 모듈을 하나의 파일로 가져오기 위한 전송 방식을 제공
- 지연 로딩(lazy loading) 을 지원

## 10.3 CommonJS

CommonJS는 서버 사이드에서 모듈을 선언하는 간단한 API를 지정하는 모듈 제안이다. AMD와는 달리 I/O, 파일시스템, 프로미스 등 더욱 광범위한 부분을 다룬다.


### 10.3.1 CommonJS 시작하기

구조적 관점에서 볼 때, CommnJS 모듈은 재사용 가능한 자바스크립트 코드로써 외부의존 코드에 공개할 특정 객체를 내보낸다. AMD와 달리 CommonJS는 모듈을 함수로 감싸는 작업이 필요하지 않다.

CommonJS는 두 가지 핵심 요소로 구성된다. 

1. exports 변수는 다른 모듈에 내보내고자 하는 객체를 담는다. 
2. require 변수 : 다른 모듈에서 내보낸 객체를 가져올 때 사용

```js
var lib = require("a/lib");

//...
exports.foo = foo;
```

### 10.3.3 Node.js 환경에서의 CommonJS(중요)

최근 ES 모듈 형식이 재사용 가능한 자바스크립트 코드를 모듈화하는 표준으로 자리 잡았지만, 여전히 Node.js 환경에서는 CommonJS가 기본형식으로 쓰인다.
Node.js 버전 13.2.0 이후의 안정적인 버전에서는 ES 모듈도 지원하기 시작했다.

기본적으로 Node.js 는 다음과 같은 파일들을 CommonJS 모듈로 인식한다.

- .cjs 확장자 파일
- 가장 가까이에 위치한 package.json 파일 안에 type이 commonjs 로 되어있을 경우
- 가장 가까이에 위치한 package.json 파일 안에 type 항목이 존재하지 않을 경우
- .mjs, .cjs, .json, .node, .js 이외의 확장자를 갖는 파일

require() 를 호출하면 항상 CommonJS 모듈 로더가 사용되고, import()를 호출하면 ECMAScript 모듈 로더가 사용된다. package.json 파일에 설정된 type 값과 관계없이 항상 적용된다.

프레임 워크에서는 Babel 같은 트랜스파일러를 사용해 가져오기/내보내기 문법을 구버전 Nodejs 에서도 작동하는 require()로 변환할 수 있다. ES6 모듈 문법으로 작성된 라이브러리는 Node.js 에서 실행할 경우 내부적으로 CommonJS로 트랜스파일 될 것이다

### 10.3.4 CommonJS는 브라우저 환경에서 적합할까?

CommonJS 모듈 구조에서는 클라이언트 서버 환경 모두에서 사용할 수 있는 모듈에는 데이터 검증, 변환, 템플릿 엔진 등이 있다.

ES2015, AMD 모듈은 생성자나 함수 같은 것을 더 세밀하게 정의할 수 있다. 반면 CommonJS는 오직 객체만을 정의할 수 있기 때문에 생성자를 정의하려는 경우 번거로운 작업이 동반될 수 있다.

## 10.4 AMD vs CommonJS 동상이몽

AMD와 CommonJS는 서로 다른 목표를 가진 모듈 형식이다.

AMD는 브라우저 우선 접근 방식을 채택하여 비동기 동작과 간소화된 하위 호환성을 선택한 반면, 파일 I/O 에 대한 개념은 없다. 브라우저 자체적으로 실행된다는 면에서 유연한 포맷이다.

CommonJS는 서버 우선 접근 방식을 취하며 동기적 작동, 전역 변수와의 독립성 그리고 미래의 서버 환경을 고려한다. CommonJS가 언래핑된 모듈을 지원하기 때문에 ES2015+ 표준에 조금 더 가깝게 느껴진다는 것이다. 이를 통해 AMD에서 필수적인 define() 함수를 사용하지 않아도 된다. 다만, CommonJS 모듈은 오직 객체만을 모듈로써 지원한다.

### 10.4.1 UMD: 플러그인을 위한 AMD 및 CommonJS 호환 모듈

UMD는 실험 단계의 모듈 포맷.
개발 당시에 존재했던 주요 스크립트 로딩 기술의 대부분을 활용해서 클라이언트 및 서버 환경 모두에서 작동하는 모듈을 구현할 수 있었다.

UMD 포맷을 만들 때 AMD 사양에서 지원하는 CommonJS 래퍼의 간소화된 방식을 참조. 

#### 기본적인 AMD 혼합 방식

```js
define(function (require, exports, module) {
	var shuffler = require("lib/shuffle");

	exports.randomize = function(input)  {
		return shuffler.shuffle(input)
	}
})
```

모듈에 의존성 배열이 없고, 정의함수가 최소한 하나 이상의 매개변수를 가질 때에만 CommonJS로써 작동한 다는 점에 유의해야한다. 또한 일부 기기에서 올바르게 작동하지 않을 수 있다.

#### CommonJS, AMD 또는 브라우저 전역 객체를 활용하여 모듈 생성하기

b라는 모듈에 의존하는 commonJsStrict 모듈을 정의해보자. 파일 명은 모듈 이름에서 유추가 가능하도록 한다.

마찬가지로 브라우저 환경에서 동일한 구조를 사용한다면, 전역 변수 .b 가 생성되어 사용된다. 전역 변수를 수정하고 싶지 않다면, 최상위 함수의 첫 인자로 전달되는 root 항목을 제거하면 된다.

```js
(function(root, factory) {
	if(typeof exports === "object"){
		factory(exports, require('b'))
	} else if(typeof define === "function" && define.amd) {
		define(["exports", "b"], factory);
	} else {
		factory((root.commonJsStrict = {}), root.b);
	}
})(this, function( exports, b ) {
	// 'b' 모듈에 정의된 항목들을 사용할 수 있다.
	
	exports.action = function() {}
})
```


