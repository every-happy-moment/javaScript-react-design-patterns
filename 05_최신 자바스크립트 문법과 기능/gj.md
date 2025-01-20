# 05. 최신 자바스크립트 문법과 기능

최신 디자인 패턴을 이해하기 위해 ES2015 버전 이상에서 지원하는 자바스크립트의 기능과 문법을 설명한다.

## 5.1 애플리케이션 분리의 중요성

애플리케이션이 모듈형이라는 것은 잘게 분리된 모듈로 구성되었음을 뜻한다. 이렇게 이루어진 느슨한 결합은 의존성을 낮추어 애플리케이션의 유지보수를 용이하게 만든다.

## 5.2 모듈 가져오기와 내보내기

## 5.3 모듈 객체

모듈을 객체로 가져오면 모듈 리소스를 깔끔하게 가져올 수 있으며, 객체 하나만으로 여러 곳에서 사용할 수 있다.

```javascript
import * as Staff from "module/staff.mjs";

Staff.baker.bake();
Staff.pastryChef.make();
```

## 5.4 외부 소스로부터 가져오는 모듈

ES2015부터 외부 소스에서 가져오는 원격 모듈을 쉽게 가져 올 수 있다.

```javascript
import { cakeFactory } from "http://example.com/modules/cakeFactory.mjs";

cakeFactory.oven.makeCupcake();
```

## 5.5 정적으로 모듈 가져오기

정적 가져오기는 메인 코드를 실행하기 전에 모듈을 다운로드하고 실행해야 한다. 따라서 초기 페이지 로드 시 많은 코드를 미리 로드해야 하므로 성능에 문제가 생길 수 있다.

## 5.6 동적으로 모듈 가져오기

지연 로딩모듈을 사용하면 필요한 시점에 로드할 수 있다.  
동적 가져오기를 사용하면 모듈이 사용될 때만 다운로드되고 실행된다.

```javascript
from.addEventListener("submit", (e) => {
  e.preventDefault();
  import("modules/cakeFactory.js").then((module) => {
    // 가져온 모듈 사용
  });
});

let module = await import("modules/cakeFactory.js");
```

## 5.7 서버에서 모듈 사용하기

> Node 15.3.0 버전 이상에서는 자바스크립트 모듈을 지원한다.

Node는 type이 module이라면 .mjs와 .js로 끝나는 파일을 자바스크립트 모듈로 취급한다.

## 5.8 모듈을 사용하면 생기는 이점

- 한 번만 실행된다.
- 자동으로 지연 로드된다.
- 유지보수와 재사용이 쉽다.
- 네임스페이스를 제공한다.
- 사용하지 않는 코드를 제거한다.(`트리 쉐이킹`)

## 5.9 생성자, getter, setter를 가진 클래스

## 5.10 자바스크립트 프레임워크와 클래스
