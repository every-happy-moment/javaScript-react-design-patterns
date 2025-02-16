# 자바스크립트 디자인 패턴

자바스크립트에서 사용되는 다양한 종류의 고전 및 최신 패턴을 설명한다.

## 7.1 생성 패턴

### 7.2 생성자 패턴

생성자는 객체가 새로 만들어진 뒤 초기화하는 데에 사용되는 특별한 메서드이다.

### 7.2.1 객체 생성

```javascript
/* 방법 1: 리터럴 표기법을 사용하여 빈객체 생성 */
const newObject = {};
/* 방법 2: Object.create() 메서드를 사용하여 빈 객체 생성 */
const newObject = Object.create(Object.prototype);
/* 방법 3: new 키워드를 사용하여 빈 객체 생성 */
const newObject = new Object();
```

### 7.2.2 생성자의 기본 특징

클래스는 새 객체를 초기화하는 constructor()라는 메서드를 가지고 있어야 한다. 또한 new키워드는 생성자를 호출할 . 수있으며, 생성자 내부에서 사용된 this 키워드는 새로 생성된 해당 객체를 가르킨다.

```javascript
class Car {
  constructor(model, year, miles) {
    this.model = model;
    this.year = year;
    this.miles = miles;
  }
  // Car 생성자로 객체를 생성할 때마다 같은 함수를 새로 정의함.
  toString() {
    return `${this.model} has done ${this.miles} miles`;
  }
}

let civic = new Car("Honda Civic", 2009, 20000);
console.log(civic.toString());
```

### 7.2.3 프로토타입을 가진 생성자

자바스크립트의 프로토타입 객체는 함수나 클래스 등 특정 객체의 모든 인스턴스 내에 공통 메서드를 쉽게 정의할 수 있게 한다.

```javascript
class Car {
  constructor(model, year, miles) {
    this.model = model;
    this.year = year;
    this.miles = miles;
  }
}
// 모든 Car 객체는 toString() 메서드를 공유함
Car.prototype.toString = function () {
  return `${this.model} has done ${this.miles} miles`;
};

let civic = new Car("Honda Civic", 2009, 20000);
console.log(civic.toString());
```

## 7.3 모듈 패턴

모듈은 애플리케이션 아키텍처의 핵심 구성 요소이며 프로젝트를 구성하는 코드 단위를 체계적으로 분리 및 관리하는데 효과적으로 활용된다.

### 7.3.1 객체 리터럴

객체 리터럴 표기법은 객체는 중괄호({}) 안에서 키와 값을 쉼표로 구분하여 객체를 정의하는 방법이다.

```javascript
const myModule = {
  myProperty: "someValue",
  myConfig: {
    useCaching: true,
  },
  saySomething() {
    // ...
  },
  reportMyConfig() {
    console.log(`${this.myConfig.useCaching}`);
  },
  updateMYConfig(newConfig) {
    if (typeof newConfig === "object") {
      this.myConfig = newConfig;
    }
  },
};
```

### 7.3.2 모듈 패턴

자바스크립트 모듈을 사용하여 객체, 함수, 클래스, 변수 등을 구성하여 다른 파일에 쉽게 내보내거나 가져올 수 있다. 이를 통해 서로 다른 모듈 간의 클래스 또는 함수명 충돌을 방지할 수 있다.

**비공개**  
모듈 패턴은 클로저를 활용해 '비공개' 상태와 구성을 캡슐화한다. 모듈 패턴을 사용한다면 공개 API만을 노출하고 나머지는 클로저 내부에 비공개로 유지할 수있다.

**역사**  
모듈 패턴은 리처트 콘포드(Richard Comford)를 비롯한 여러 사람들에 의해 발명되었다. 이후 더글라스 클락포드(Douglas Crockford)가 자신의 강의에서 언급하며 대중화되었다.

**예제**  
export는 모듈 외부에서 모듈 기능에 대한 액세스를 제공하는 역할을 한다. 그리고 import는 모듈에서 내보낸 바인딩을 가져올 수 있게 한다.

```javascript
// 비공개 변수 및 함수
const basket = [];
const doSomethingPrivate = () =>{
    // ...
}
// 다른 파일에 공개할 객체 생성
const basketModule = {
    addItem(values){
        basket.push(values);
    }
    // 비공개 함수를 공개 함수로 감싸 다른 이름으로 사용하기
    doSomething(){
        doSomethingPrivate();
    }
}
export default basketModule;

// 경로로부터 모듈 가져오기
import  basketModule from './basketModule';

basketModule.addItem({
    item:'bread'
})

// undefined ; // 비공개
console.log(basketModule.basket);
```

### 7.3.3 모듈 패턴의 변형

**믹스인(Mixin) 가저오기 변형**  
유틸 함수나 외부 라이브러리 같은 전역 스코프에 있는 요소를 모듈 내부의 고차 함수에 인자로 전달 할 수 있게 한다.

**내보내기 변형**  
따로 이름을 지정해주지 않고 전역 스코프로 변수를 내보낸다.

**장점**

- 객체 지향 프로그래밍 지식을 가진 초보 개발자가 이해하기 쉽다.
- 비공개를 지원한다.
- 공개되면 안 되는 코드를 캡슐화할 수 있다.

**단점**

- 공개와 비공개 멤버를 서로 다르게 접근해야 한다.
- 나중에 추가한 메서드에서는 비공개 멤버에 접근할 수 없다.
- 자동화 단위 테스트에서 비공개 멤버는 제외된다는 것과 핫픽스가 필요한 오류를 고찰 때 복잡도를 높인다.

### 7.3.4 WeakMap을 사용하는 최신 모듈 패턴

WeakMap객체는 키가 약하게 유지되는 Map이다. 즉 참조되지 않는 키는 가비지 컬렉션의 대상이 된다.

### 7.3.5 최신 라이브러리와 모듈

## 7.4 노출 모듈 패턴

모든 함수와 변수를 비공개 스코프에 정의하고 공개하고 싶은 부분만 포인터를 통해 비공개 요소에 접근할 수 있게 해주는 익명 객체를 반환하는 패턴이다. 노출 모듈 패턴을 사용하면 좀 더 구체적인 이름을 붙여 비공개 요소를 공개로 내보낼 수도 있다.

```javascript
let privateVar = "Rob Dodson";
const publicVar = "Hey there!";

const privateFunction = () => {
  console.log(`Name:${privateVar}`);
};

const publicGeName = () => {
  privateFunction();
};
const myRevealingModule = {
  getName: publicFunction,
  greeting: publicVar,
};
export default myRevealingModule;

import myRevealingModule from "./myRevealingModule";
myRevealingModule.getName();
```

### 7.4.1 장점

- 코드의 일관성이 유지된다.
- 모듈의 가장 아래에 위치한 공개 객체를 더 알아보기 쉽게 바꾸어 가독성을 향상시킨다.

### 7.4.2 단점

- 비공개 함수를 참조하는 공개 함수를 수정할 수 없다.
  - 비공개 함수가 비공개 구현을 참조하기 때문에 발생하며, 수정을 해도 함수가 변경될 뿐 참조된 구현이 변경되는 것이 아니다.
- 기존 모듈 패턴보다도 취약할 수 있으므로 사용에 주의해야 한다.

## 7.5 싱글톤 패턴

싱글톤 패턴은 클래스의 인스턴스가 오직 하나만 존재하도록 제한하는 패턴이다. 전역에서 접근 및 공유해야 하는 단 하나의 객체가 필요할 때 유용하다.  
싱글톤 패턴은 초기화를 지연시킬 수 있다. (초기화 시점에 필요한 특정 정보가 유효하지 않을 수도 있기 때문)

싱글톤은 인스턴스에 대한 전역 접근을 허용한다.

```javascript
let instance;

const randomNumber = Math.random();

class MySingleton {
  constructor() {
    if (!instance) {
      instance = this;
    }
    return instance;
  }

  getRandomNumber() {
    return randomNumber;
  }
}
export default MySingleton;

import MySingleton from "./MySingleton";
const singleA = new MySingleton();
const singleB = new MySingleton();
console.log(singleA.getRandomNumber() === singleB.getRandomNumber()); //true
```

- 싱글톤의 적합성
  - 클래스의 인스턴스는 정확히 하나만 있어야 하며 눈에 잘 보이는 곳에 위치시켜 접근을 용이하게 해야 한다.
  - 싱글톤의 인스턴스는 서브클래싱을 통해서만 확장할 수 있어야 하고, 코드의 수정없이 확장된 인스턴스를 사용할 수 있어야 한다.
- javascript에서 싱글톤 클래스 사용하면 발생하는 단점
  - 싱글톤임을 파악하는 것이 힘들다.
  - 테스트 하기 힘들다.
  - 신중한 조정이 필요하다.

### 7.5.1 리액트의 상태 관리

리액트를 통해 개발을 한다면 싱글톤 대신 context API나 리덕스 같은 전역 상태 관리 도구를 이용하여 개발할 수 있다. 전역 상태 관리 도구는 변경 불가능한 읽기 전용 상태를 제공한다. (컴포넌트가 전역 상태를 직접 변경할 수 없게 만들어 전역 상태가 의도한 대로 변경될 수 있도록 도와준다.)

## 7.6 프로토타입 패턴

프로토타입 패턴은 프로토타입의 상속을 기반으로 한다. 이 패턴에서는 프로토타입 역할을 할 전용 객체를 생성하게 된다.

프로토타입 패턴의 장점은 다른 언어의 기능을 따라하지 않고, 자바스크립트만이 가진 고유의 방식으로 작업할 수 있다는 것이다. 또한 상속을 구현하는 쉬운 방법이며, 모든 객체가 동일한 함수를 가리키게 할 수 있기 때문에 성능에서의 이점도 챙길 수 있다.

Object.create는 프로토타입 객체를 생성하고 특정 속성을 추가할 수 있다 또한 다른 객체로부터 직접 상속할 수있게 해주는 차등 상속과 같은 고급 개념을 쉽게 구혈할 수있게 해준다.

```javascript
const vehicle = {
    getModel(){
        console.log(`this model of this vehicle is...${this.model}`)
    }
}
const car = Object.create(vehicle,{
  {
    model:{
        value:'Ford',
        enumerable:true,
    }
  }
})
```

## 7.7 팩토리 패턴

팩토리 패턴은 객체를 생성하는 생성패턴이다. 동적인 요소나 애플리케이션 구조에 깊게 의지하는 등의 상황처럼 객체 생성과정이 복잡할 때 유용하다.

### 7.7.1 팩토리 패턴을 사용하면 좋은 상황

- 객체나 컴포넌트의 생성 과정이 높은 복잡성을 가지고 있을 때
- 상황에 맞춰 다양한 객체 인스턴스를 편리하게 생성할 수 있는 방법이 필요할 때
- 같은 속성의 공유하는 여러 개의 작은 객체 또는 컴포넌트를 다뤄야 할 때
- 덕 타이핑 같은 API규칙만 충족하면 되는 다른 객체의 인스턴스와 함께 객체를 구성할 때

### 7.7.2 팩토리 패턴을 사용하면 안되는 상황

팩토리 패턴은 객체 생성 과정을 인터페이스 뒤에 추상화하기 때문에 객체 생성 과정이 복잡할 경우 단위 테스트의 복잡성 또한 증가시킬수 있다.

## 7.7.3 추상 팩토리 패턴

추상 팩토리 패턴은 같은 목표를 가진 각각의 팩토리들을 하나의 그룹으로 캡슐화하는 패턴이다. 또한 객체가 어떻게 생성되는지에 대한 세부사항을 알 필요 없이 객체를 사용할 수있게 한다.

객체의 생성 과저엥 영향을 받지 않아야 하거나 여러 타입의 객체로 작업해야 하는 경우에 추상 팩토리를 사용하면 좋다.

## 7.8 구조 패턴

구조 패턴은 클래스와 객체의 구성을 다룬다.

- 퍼사드 패턴
- 믹스인 패턴
- 데코레이터 패턴
- 플라이웨이트 패턴

## 7.8. 퍼사드 패턴

> 퍼사드(Facade)란, 실제 모습을 숨기고 꾸며낸 겉모습만을 세상에 드러내는 것을 뜻함.

퍼사드 패턴은 심층적인 복잡성을 숨기고, 사용하기 편리한 높은 수준의 인터페이스를 제공하는 패턴이다.

## 7.10 믹스인 패턴

서브클래스가 쉽게 상속받아 기능을 재사용할 수 있도록 하는 클래스이다.

## 7.11 서브클래싱

부모 클래스를 확장하는 자식 클래스를 서브클래스라고 한다.

서브 클래싱이란 부모 클래스 객체에서 속성을 상속받아 새로운 객체를 만드는 것을 뜻한다. 서브클래스는 부모 클래스에서 먼저 정의된 메서드를 오버라이드 하는 것도 가능하다.

> 메서드 체이닝: 서브클래스의 메서드가 오버라이드된 부모 클래스의 메서드를 호출하는 것
> 생성자 체이닝: 서브클래스의 메서드가 부모클래스의 생성자를 호출하는 것

## 7.12 믹스인

믹스인은 최소한의 복잡성으로 객체의 기능을 빌리거나 상속할 수 있게 해준다. 또한 다른 여러 클래스를 아울러 쉽고 공유할 수 있는 속성과 메서드를 가진 클래스이다.

## 7.12.1 장점과 단점

장점: 믹스인은 함수의 중복을 줄이고 재사용성을 높인다.
단점: 객체의 프로토타입에 기능을 주입하면 프로토타입 오염과 함수의 출처에 대한 불확실성을 초래한다.

## 7.13 데코레이터 패턴

데코레이터 패턴은 코드 재서용을 목표로 하는 구조 패턴이며, 객체 생성을 신경 쓰지 않는 대신 기능의 확장에 좀 더 초점을 둔다.

데코레이터를 사용하면 기존 시스템의 내부 코드를 힘겹게 바꾸지 않고도 기능을 추가할 수 있게 된다.

## 7.14 의사 클래스 데코레이터

더스틴 디아즈(Dustin Diaz)와 로스 하메스(Ross Harmes)가 선보인 데코레이터의 변형 버전에 대해 설명한다.

## 7.14.1 인터페이스

데코레이터 페턴을 같은 인터페이스를 가진 서로 다른 객체 내부에 새 객체를 넣어서 사용하는 방법이라고 설명한다.

**인터페이스**란 객체가 가져야 할 메서드를 정의하는 방법이다.

## 7.17 행위 패턴

행위 패턴은 객체 간의 의사소통을 돕는 패턴이다.

- 관찰자 패턴
- 중재자 패턴
- 커맨드 패턴

## 7.20 커맨드 패턴

커맨드 패턴은 메서드 호출, 요청 또는 작업을 단일 객체로 캡슐화하여 추후에 실행할 수 있도록 해준다. 또한 명령을 실행하는 객체와 명령을 호출하는 객체 간의 결합을 느슨하게 하여 구체적인 클래스의 변경에 대한 유연성을 향상시킨다.

커맨드 패턴의 기본 원칙은 명령을 내리는 객체와 명령을 실행하는 객체의 책임을 분리한다는 것이다. 이러한 책임을 다른 객체에 위임함으로써 역할 분리를 실현한다.

```javascript
const CarManager = {
  requestInfo(model, id) {
    return `this information for ${model} with ID ${id} is foobar`;
  },
  buyVehicle(model, id) {
    return `You have successfully purchased Item ${id}, a ${model}`;
  },
  arrangeViewing(model, id) {
    return `You have booked a viewing of ${model} (${id})`;
  },
};
CarManager.execute = function (name) {
  return (
    CarManger[name] &&
    CarManager[name].apply(CarManager, [].slice.call(arguments, 1))
  );
};
```
