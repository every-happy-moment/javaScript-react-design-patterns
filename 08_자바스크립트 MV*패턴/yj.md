# 08_자바스크립트 MV* 패턴

## 8.1 MVC 패턴
MVC 패턴은 애플리케이션의 구조를 개선하기 위해 관심사의 분리를 활용하는 아키텍처 디자인 패턴이다. 비즈니스 데이터(모델)과 UI(뷰)를 분리하고, 세 번째 구성 요소(컨트롤러)가 로직과 사용자 입력을 관리하는 구조이다. 

### 8.1.1 Smalltalk-80의 MVC 패턴
- 모델(Model): 도메인 관련 데이터를 표현했으며 UI(뷰와 컨트롤러)에 대해서는 관여하지 않았다. 모델이 변경되면 자신의 관찰자(observer) 객체에게 알림을 보냈다.
- 뷰(View): 모델의 현재 상태를 표현했다. 관찰자 패턴을 사용해 모델이 변경되거나 수정될 때마다 뷰가 알아차릴 수 있도록 했다. 또한 뷰는 화면에 보여지는 프레젠테이션 부분만을 담당했지만, 화면에 표시되는 각 섹션 또는 요소에는 언제는 뷰-컨트롤러 쌍이 존재했다.
- 컨트롤러(Controller): 키보드 입력이나 클릭 같은 사용자의 상호작용을 처리하고 뷰에 무엇을 보여줄지, 사용자 입력을 어떻게 처리할지 등을 결정하는 역할을 했다.

## 8.2 자바스크립트의 MVC
구조의 결함 때문에 읽기가 어렵고 유지 관리하기 힘든 코드를 뜻하는 '스파게티 코드'를 피하는 것이 중요하다는 점을 고려할 때, 현대 자바스크립트 개발자는 MVC 패턴과 그 변형 버전들이 제공하는 장점을 이해해야 한다. 이를 통해 프레임워크를 활용했을 때 달라지는 개발 방식을 효과적으로 파악할 수 있다.

### 8.2.1 모델
모델은 애플리케이션의 데이터를 관리하는 역할을 한다. UI나 프레젠테이션 계층은 담당하지 않고, 애플리케이션에 필요한 고유 데이터 형식을 나타낸다. 모델이 변경될 때(예: 업데이트) 관찰자(예: 뷰)에게 변경사항을 알린다. 이렇게 함으로써 관찰자가 변경된 내용에 알맞게 능동적으로 대응할 수 있게끔 한다. 정리하자면, 모델은 비즈니스 데이터와 주로 관련이 있다.

### 8.2.2 뷰
뷰는 모델에 대한 시각적인 표현으로, 현재 상태의 특정 부분만 보여준다. 자바스크립트의 뷰는 여러 DOM 요소의 집합을 생성하고 정리하는 역할을 한다.<br />
일반적으로 뷰는 모델을 관찰하고 모델에 변화가 생기면 알림을 받는다. 이를 통해 뷰는 스스로를 업데이트할 수 있다.<br />
사용자는 뷰와 상호작용 할 수 있다. 뷰는 프레젠테이션 계층이기 때문에 편집과 업데이트 기능을 유저 친화적으로 제공한다.<br />
모델을 실제로 업데이트하는 작업은 컨트롤러가 담당한다. 사용자가 뷰 내의 요소를 클릭하면, 뷰는 그 다음에 무엇을 해야할지 알지 못한다. 대신 뷰는 이 결정을 컨트롤러에 위임한다. 

### 8.2.3 템플릿
최신 자바스크립트 템플릿 솔루션은 ES6의 강력한 기능인 태그 템플릿 리터럴(tagged template literals)의 사용으로 방향을 전환했다. 태그 템플릿 리터럴을 사용하면 자바스크립트 템플릿 리터럴 문법과 함께 템플릿을 조작하고 데이터를 채우는 데 사용할 수 있는 커스텀 처리 함수를 통해 재사용 가능한 템플릿을 만들 수 있다. 이 접근 방식은 별도의 템플릿 라이브러리의 필요성을 제거하고, 동적인 HTML 콘텐츠를 생성하는 간결하고 유지보수가 용이한 방법을 제공한다.<br />
태그 템플릿 리터럴 내의 변수는 ${variable} 구문을 사용하여 쉽게 추가할 수 있다.<br />
템플릿 자체가 뷰는 아니다. 뷰는 모델을 관찰하고 시각적 표현(UI)을 최신 상태로 유지하는 객체이다. 프레임워크가 템플릿 명세에 따라 뷰를 생성할 수 있도록, 템플릿은 뷰 객체의 일부 또는 전체를 선언적으로 지정하는 방법이 될 수 있다.

### 8.2.4 컨트롤러
컨트롤러는 모델과 뷰 사이의 중재자 역할을 하며, 일반적으로 사용자가 뷰를 조작할 때 모델을 업데이트하는 역할을 한다. 컨트롤러는 애플리케이션 내에서 모델과 뷰 간의 로직 그리고 연동을 관리한다.

## 8.3 MVC를 사용하는 이유는?
- 전반적인 유지보수의 단순화: 애플리케이션을 업데이트해야 할 때, 변경사항이 데이터 중심인지 (모델과 컨트롤러의 변경) 아니면 단순히 시각적인 변경인지 (뷰의 변경) 명확하게 구분할 수 있다.
- 모델과 뷰의 분리: 비즈니스 로직에 대한 단위(unit) 테스트의 작성이 훨씬 간편해진다.
- 애플리케이션 전반에서 하위 수준의 모델 및 컨트롤러 코드 중복이 제거된다.
- 애플리케이션의 규모와 역할의 분리 정도에 따라, 모듈화를 통해 코어 로직을 담당하는 개발자와 UI 작업을 담당하는 개발자가 동시에 작업할 수 있다.

## 8.6 MVP 패턴
MVP(Model-View-Presenter) 패턴은 프레젠테이션 로직의 개선에 초점을 맞춘 MVC 디자인 패턴의 파생이다. MVC와 MVP 모두 여러 구성 요소 간의 관심사 분리를 목표로 하지만, 몇 가지 근본적인 차이점이 있다.

### 8.6.1 모델, 뷰, 프리젠터
MVP에서 P는 프리젠터(presenter)를 의미한다. 프리젠터는 뷰에 대한 UI 비즈니스 로직을 담당하는 구성 요소이다. MVC와 달리, 뷰에서의 이벤트 호출을 프리젠터로 위임된다. 프리젠터는 뷰와 분리되어 있으며, 인터페이스를 통해 뷰와 통신한다. 이 방식은 단위 테스트에서 뷰를 모킹(mocking)할 수 있는 등의 많은 장점을 제공한다.<br />
MVC에서 MVP로의 변화는 애플리케이션의 테스트 용이성을 높이고 뷰와 모델 간의 분리를 더욱 명확하게 해준다는 장점이 있다. 그러나 MVP 패턴에는 데이터 바인딩이 지원되지 않기 때문에, 작업을 별도로 처리해야 하는 비용이 발생할 수 있다.

### 8.6.2 MVP vs MVC
MVP는 일반적으로 프레젠테이션 로직을 최대한 재사용해야 하는 엔터프라이즈 수준의 애플리케이션에서 사용된다. 뷰가 매우 복잡하고 사용자와의 상호작용이 많은 애플리케이션에서는 MVC가 적합하지 않을 수 있다. 이런 문제를 MVC로 해결하려면 여러 컨트롤러에 크게 의존해야 할 수 있기 때문이다. MVP 에서는 이 모든 복잡한 로직을 프리젠터 안에 캡슐화 할 수 있어 유지보수가 훨씬 간단해진다.

## 8.7 MVVM 패턴
MVVM(Model-View-ViewModel) 패턴은 MVC와 MVP를 기반으로 하는 아키텍처 패턴으로, 애플리케이션의 UI 개발 부분과 비즈니스 로직, 동작 부분을 명확하게 분리한다. 많은 MVVM의 구현 방식은 선언적 데이터 바인딩을 활용하여 뷰에 대한 작업을 다른 계츨과 분리할 수 있도록 한다. 이러한 기법을 통해 동일한 코드베이스 내에서 UI 작업과 개발 작업을 거의 동시에 진행할 수 있다. 

### 8.7.2 모델
다른 MV* 패턴들과 마찬가지로, MVVM의 모델은 우리 애플리케이션이 사용할 도메인 관련 데이터나 정보를 제공한다. 모델은 보통 정보를 담고 있지, 동작을 다루지 않는다. 모델은 정보 형식을 지정하지 않고, 데이터가 브라우저에 어떻게 표현될지에 영향을 미치지 않는다. 데이터 형식 지정은 뷰가 담당하고, 동작은 모델과 상호작용하는 또 다른 계층인 뷰모델에서 캡슐화하여 처리해야 하는 비즈니스 로직으로 간주된다.<br />
모델의 역할 중 유일한 예외는 데이터 유효성 검사이다. 기존 모델을 정의하거나 업데이트 하는 데 사용되는 데이터에 대한 유효성 검사는 모델에서 수행하는 것이 허용된다.(예: 입력된 이메일 주소가 특정 정규식의 요구 조건을 충족하는가?)

### 8.7.3 뷰
MVC와 마찬가지로, 뷰는 애플리케이션에서 사용자가 상호작용하는 유일한 부분이고, 뷰모델의 상태를 표현하는 상호작용이 가능한 UI이다. 중요한 점은 뷰는 상태를 관리할 책임이 없다는 것이다. 뷰는 뷰모델과 정보 또는 상태를 항상 동기화된 상태로 유지하기 때문이다.

### 8.7.4 뷰모델
뷰모델(ViewModel)은 데이터 변환기의 역할을 하는 특수한 컨트롤러로 볼 수 있다. 모델의 정보를 뷰가 사용할 수 있는 형태로 변환하고, 뷰에서 발생한 명령(사용자의 조작이나 이벤트)을 모델로 전달한다.<br />
이러한 관점에서, 뷰모델은 뷰라기보다는 모델에 더 가깝다고 볼 수 있다. 그러나 동시에 뷰의 디스플레이 로직 대부분을 처리한다.<br />
정리하자면, 뷰모델은 UI 계층의 뒤에 위치한다. 뷰가 필요로 하는 데이터를 (모델로부터 가져와) 제공하며, 데이터와 사용자의 동작 모두를 뷰가 참조하는 출처의 역할을 할 수 있다.

### 8.7.5 뷰와 뷰모델 복습
뷰와 뷰모델은 데이터 바인딩과 이벤트를 통해 소통한다. 뷰모델은 모델의 속성을 단순히 제공하는 것뿐만 아니라, 데이터 유효성 검사같은 다른 메서드와 기능에 대한 접근도 허용한다.<br />
뷰는 자체 UI 이벤트를 처리하고, 필요에 따라 뷰모델에 연결(mapping)한다. 모델과 뷰모델의 속성은 양방향 데이터 바인딩을 통해 동기화되고 업데이트 된다.

### 8.7.6 뷰모델 vs 모델
뷰모델은 데이터 바인딩을 위해 모델 또는 모델으 속성을 가져올 수 있고, 뷰에 제공되는 속성을 가져오거나 조작하기 위한 인터페이스를 포함할 수 있다.

## 8.8 장단점
### 8.8.1 장점
- MVVM은 UI와 이를 구동하게 해주는 요소를 동시에 개발할 수 있도록 한다.
- MVVM은 뷰를 추상화함으로써 뷰의 뒤에 작성되는 비즈니스 로직(또는 연결 코드)의 양을 줄여준다.
- 뷰모델은 이벤트 중심 코드에 비해 단위 테스트가 더 쉽다.
- 뷰모델은 (뷰보다는 모델에 가까우므로) UI 자동화나 상호작용에 대한 고려 없이도 테스트가 가능하다.

### 8.8.2 단점
- 단순한 UI의 경우, MVVM은 과도한 구현(overskill)이 될 수 있다.
- 데이터 바인딩은 선언적이고 사용하기 편리할 수 있지만, 단순히 중단점(breakpoints)을 설정하는 명령형 코드에 비해 디버깅이 더 어려울 수 있다.
- 복잡한 애플리케이션에서는, 데이터 바인딩이 상당한 관리 부담을 만들어 낼 수 있다. 또한 바인딩 코드가 바인딩 대상 객체보다 더 무거운 상황도 피하고 싶다.
- 대규모 애플리케이션에서는 필요한 일반화를 제공하기 위해 뷰모델을 미리 설계하는 것이 어려울 수 있다.

## 8.9 MVC vs MVP vs MVVM
MVP와 MVVM은 모두 MVC에서 파생된 패턴이다. MVC와 이 파생 패턴들 사이의 핵심 차이점은 각 계층이 다른 계층에 대해 갖는 의존성과, 서로 얼마나 강하게 연결되어 있는지에 있다.

## 8.10 최신 MV* 패턴
### 8.10.1 MV* 패턴과 리액트
리액트는 MVC 프레임워크가 아니다. 리액트는 UI 구축을 위한 자바스크립트 라이브러리이며, 주로 SPA(Single Page Application)개발에 사용된다.<br />
리액트는 백엔드에서 전통적으로 구현되고, 사용되는 MVC 패턴과 잘 맞지 않기 때문에 MVC로 분류되지 않는다. 리액트는 뷰 계층을 원하는대로 구성하게 해주는 렌더링 라이브러리이다. 기존 MVC와 같이 중앙 제어 역할을 하는 컨트롤러, 혹은 라우터 기능이 포함되어 있지 않다.<br />
리액트를 MVC 디자인 패턴에서 사용하지 않는 이유는 리액트에서는 서버가 브라우저에 '뷰'를 직접 제공하지 않고, '데이터'를 제공하기 때문이다.<br />
리액트와 마찬가지로 Next.js 도 MVC 프레임워크는 아니지만, 서버 사이드 렌더링(SSR) 또는 정적 사이트 생성(Static Site Generation(SSG))을 사용하는 경우 MVC와 유사한 패턴으로 동작할 수 있다. Next.js가 백엔드 역할을 수행하여 데이터베이스와 상호작용하고 뷰를 사전 렌더링하면, 이후부터는 리액트의 반응형 기능을 통해 뷰를 동적으로 업데이트함으로써 전통적인 MVC 형태로 동작한다.

## 8.11 마치며
이러한 기본 패턴의 이해는 전체 웹 애플리케이션의 아키텍처를 설계하는데 기초적 가이드라인을 제공할 수 있다. 또한 애플리케이션을 수직 분할하여 다수의 컴포넌트로 구성할 때, 개별 프론트엔드 컴포넌트에 뷰모델이나 모델의 개념을 적용하여 컴포넌트의 뷰를 구성하는 데에 도움을 줄 수 있다.
