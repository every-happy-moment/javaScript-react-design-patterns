# 14장 리액트 애플리케이션 구조

## 14.1 소개

React 자체는 프로젝트 구조에 대한 가이드라인을 제공하지 않지만, 주로 사용되는 몇 가지 접근 방식이 있다. 

- 기능별 그룹화(Feature)
	- 각 애플리케이션 모듈, 기능 또는 경로별로 폴더를 만든다.
- 파일 유형병 그룹화(Type)
	- CSS, JS, 이미지 등 파일 유형별로 폴더를 만든다.


### 14.1.1 모듈 기능 또는 경로별 그룹화

파일구조는 비즈니스 모델이나 애플리케이션 흐름을 반영한다.

```
common/
	Avatar.js
	Avatar.css
	ErrorUtls.js
	ErrorUtls.test.js
product/
	index.js
	product.css
	price.js
	product.test.js
checkout/
	index.js
	checkout.css
	checkout.test.js
```

기능별로 파일을 그룹화하면 모듈에 변경사항이 있을 때 관련된 모든 파일이 같은 폴더에 모여 있어 변경사항이 특정 코드 영역으로 제한된다는 장점이 있다.
하지만, 반대로 모듈간에 공통적으로 사용되는 컴포넌트, 로직 또는 스타일을 주기적으로 파악해야만 중복을 피하고 일관성과 재사용성을 높일 수 있다는 단점도 있다.


### 14.1.2 파일 유형별 그룹화

CSS, 컴포넌트, 테스트파일, 이미지, 라이브러리 등 파일 유형별로 서로 다른 폴더를 생성하는 것이다. 따라서 논리적으로 관련된 파일들이 파일 유형에 따라 서로 다른 폴더에 위치하게 된다.

```
css/
	global.css
	checkout.css
	product.css
lib/
	date.js
	currency.js
	gtm.js
gages/
	checkout.js
	product.js
	productList.js	
```

애플리케이션의 기능이 많아질 수 록 각 폴더의 파일 수가 증가하여 특정 파일을 찾기 어려워질 수 있다.


### 14.1.3 도메인 및 공통 컴포넌트 기반의 혼합 그룹화

도메인 및 공통 컴포넌트 기반의 혼합(Hybrid) 그룹화 방식에서는 애플리케이션 전체에서 공통적으로 사용되는 컴포넌트들을 모두 Components 폴더에 그리고 애플리케이션 흐름에 특화된 경로나 기능들을 모두 domain 폴더(또는 pages나 routes) 에 그룹화한다.

```
css/
components
	User/
		prifile.js
		profile.test.js
		avatar.js
	date.js
	gtm.js
	errorUitls.js
domain/
	product/
		product.js
		product.css
		product.test.js
	checkout/
		checkout.js
		checkout.css
		checkout.test.js		
```


> 3~4 단계 이상의 깊은 중펍은 피하는 것이 좋다. 폴더 간의 상대 경로를 작성하거나 파일 이동 시 경로를 업데이트하기 어려워지기 때문이다.

## 14.2 최신 리액트 기능을 위한 애플리케이션 구조


### 14.2.1 리덕스

리덕스 공식 문서에서는 특정 기능에 대한 로직을 한 곳에 모아두는 것을 강력하게 권장한다. 특정 기능 폴더 내에서 해당 기능의 리덕스 로직은 단일 슬라이스 파일로 작성되어야하며, 가급적이면 Toolkit을 사용하는 것이 좋다.



### 14.2.2 컨테이너

프레젠테이션 컴포넌트와 상태 저장 컨테이너 컴포넌트로 분류하는 방식으로 코드를 구성했다면, 컨테이너 컴포넌트를 위한 별도의 폴더를 만들 수 있다.


### 14.2.3 Hooks

다른 유형의 코드와 마찬가지로 혼합 구조에 잘 어울린다. 단일 컴포넌트에서만 사용되는 Hooks는 해당 컴포넌트의 파일이나 컴포넌트 폴더 내 별도의 hooks.js파일에 유지해야한다.
