# 14_리액트 애플리케이션 구조
## 14.1 소개
리액트 애플리케이션에서 파일을 그룹화하는 방법은 크게 두 가지가 있다.
- 기능별 그룹화: 각 애플리케이션 모듈, 기능 또는 경로별로 폴더를 만든다.
- 파일 유형별 그룹화: CSS, 자바스크립트, 이미지 등 파일 유형별로 폴더를 만든다.

### 14.1.1 모듈, 기능 또는 경로별 그룹화
모듈, 기능 또는 경로별 그룹화의 경우, 파일 구조는 비즈니스 모델이나 애플리케이션의 흐름을 반영한다. 기능별로 파일을 그룹화하면, 모듈에 변경사항이 있을 때 관련된 모든 파일이 같은 폴더에 모여 있어 변경사항이 특정 코드 영역으로 제한된다는 장점이 있다. <br />
하지만 반대로 모듈 간에 공통적으로 사용되는 컴포넌트, 로직 또는 스타일을 주기적으로 파악해야만 중복을 피하고 일관성과 재사용성을 높일 수 있다는 단점도 있다.

### 14.1.2 파일 유형별 그룹화
파일 유형별 그룹화 방식은 CSS, 컴포넌트, 테스트 파일, 이미지, 라이브러리 등 파일 유형별로 서로 다른 폴더를 생성하는 것이다. 따라서 논리적으로 관련된 파일들이 파일 유형에 따라 서로 다른 폴더에 위치하게 된다. <br />
이 폴더 구조의 장점은 다음과 같다.
- 표준 구조 재사용: 여러 프로젝트에서 동일하게 사용할 수 있는 표준적인 구조를 갖는다.
- 새로운 팀원의 빠른 적응: 애플리케이션별 로직에 대한 지식이 부족한 신규 팀원도 스타일이나 테스트 파일을 쉽게 찾을 수 있다.
- 공통 컴포넌트 및 스아리 변경 용이
<br /><br />
단점은 다음과 같다.
- 모듈 수정시 여러 폴더 수정 필요
- 파일 찾기 어려움
<br /><br />
각 구조는 폴더당 파일 수가 적은(50~100개) 중소 규모 애플리케이션에서는 설정하기 쉽다. 하지만 대규모 프로젝트의 경우, 애플리케이션의 논리 구조에 따라 혼합된 접근 방식을 사용하는 것이 좋다.

### 14.1.3 도메인 및 공통 컴포넌트 기반의 혼합 그룹화
도메인 및 공통 컴포넌트 기반의 혼합 그룹호 ㅏ방식에서는 애플리케이션 전체에서 공통적으로 사용되는 컴포넌트들을 모두 Components 폴더에, 그리고 애플리케이션 흐름에 특화된 경로나 기능들을 모두 domain 폴더(또는 pages나 routes)에 그룹화한다. 각 폴더에는 특정 컴포넌트 및 관련 파일들으 위한 하위 폴더가 있을 수 있다.<br />
이렇게 하면 '파일 유형별 그룹화'와 '기능별 그룹화'의 장점을 모두 활용하여, 자주 변경되는 관련 파일들을 한 곳에 모으면서도, 애플리케이션 전체에서 공통적으로 상요되는 재사용 가능한 컴포넌트와 스타일도 함께 관리할 수 있다.
```javascript
// 중첩된 구조의 예
domain/
    product/
        productType/
            features.js
            features.css
            size.js
        price/
            listprice.js
            discount.js
```
Note: 3~4단계 이상의 깊은 중첩은 피하는 것이 좋다. 폴더 간의 상대 경로를 작성하거나 파일 이동시 경로를 업데이트하기 어려워지기 때문이다.

## 14.2 최신 리액트 기능을 위한 애플리케이션 구조
## 14.3 기타 모범 사례
- Import aliasing 사용: 공통 가져오기에 대한 긴 상대 경로를 줄이기 위해 import aliasing을 사용한다. Babel 및 웹팩 설정을 통해 구현할 수 있다.
- 외부 라이브러리 API로 감싸기: 필요한 경우 교체할 수 있도록 외부 라이브러리를 자체 API로 감싸서 사용한다.
- ProtoTypes 사용: 컴포넌트에 ProtoTypes를 사용하여 속성 값의 유형 검사를 수행한다.