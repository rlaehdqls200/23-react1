# 201930403 김동빈

## 2023.03.23(4주차)
### JSX란 무엇인가? -  [공식 문서](https://ko.reactjs.org/docsintroducing-jsx.html)<br>
1. JavaScript를 확장한 문법(JavaScript에 모든 기능이 포함 돼있다, 변형 된게 아니다.)
1. 표현식이다.
    - 컴파일 종료 후 JSX 표현식이 정규 JavaScript 함수 호출 후 JavaScript 객체로 인식된다.
    - 즉, JSX를 if 구문 및 for loop 안에 사용하고,<br> 변수에 할당하고, 인자로서 받아들이고, 함수로부터 반환할 수 있다.
### JSX의 특징
1. JSX로 작성된 코드는 모두 자바스크립트 코드로 변환
1. 태그가 비어있다면 XML처럼 /> 를 이용해 바로 닫아야 한다.  
1. JSX 태그는 자식을 포함할 수 있다.
1. 어트리뷰트에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.  
    - ex) ```const element = <a href="https://www.reactjs.org"> link </a>;```
1. 중괄호를 사용하여 어트리뷰트에 JavaScript 표현식을 삽입할 수 있다.
    - ex) ```const element = <img src={user.avatarUrl}></img>;```
1. <b>객체 표현</b>이 가능하다, Babel은 JSX를 React.createElement() 호출로 <b>컴파일</b>한다.
    - React.createElement()는 버그가 없는 코드를 작성하는 데 도움이 되도록 검사 한다.

### JSX의 장점
1. 코드가 간결해 진다.
1. 가독성이 향상 된다.
1. Injection Attack이라 불리는 해킹 방법을 방어함으로써 보안에 강하다.

### JSX 사용법
1. 기본적으로 모든 자바스크립트 문법을 지원
1. 자바스크립트에 XML과 HTML을 섞어서 사용
1. 중괄호를 사용하여 자바스크립트 코드를 삽입

***
***

## 2023.03.16(3주차)
###  리액트란 무엇인가 ?<br>
**React** : 사용자 인터페이스를 만들기 위한 <u>자바스크립트 라이브러리</u><br>
라이브러리 : 누군가가 미리 만들어둔 기능들을 모아둔 것.<br>
- 자바스크립트에서 코딩을 하다 리액트라는 라이브러리를 import 하는게 아닌,<br> 
자바스크립트랑 상관없이 리액트에서 코딩을 하는 특수한 라이브러리 케이스다.<br>

복잡한 사이트를 쉽고 빠르게 만들고, 관리하기 위해 만들어진 것이 바로 **리액트**<br>
(다른 표현으로 SPA를 쉽고 빠르게 만들 수 있도록 해주는 도구라고 생각하면 된다.)
### 리액트의 장점
1. 빠른 업데이트와 비동기식 렌더링 – **Virtual DOM**를 사용<br>
    - Virtual DOM : DOM(Document Object Model)에 단점을 보완한 인터페이스.
    - 동기식, 비동기식 개념<br>
    동기식 : A와 B가 같이 움직인다. 예) 요청하면 요청 부분 포함 통째로 만들어서 준다.<br>
    비동기식 : A와 B가 따로 움직인다. 예) 요청하면 요청 부분만 만들어서 준다.
1. 컴포넌트 기반 구조로 재사용성이 뛰어나다.  
    - 컴포넌트(Component) :<br> 여러 개의 함수들을 모아 하나의 특정 기능을 수행할 수 있도록 구성한 작은 기능적 단위
    - 리액트의 모든페이지는 컴포넌트로 구성된다.  
    - 하나의 컴포넌트는 다른 여러 개의 컴포넌트의 조합으로 구성할 수 있다.  
    - 레고 블록을 조립하는 것처럼 컴포넌트를 조합해서 웹사이트를 개발하게 된다.
1. 재사용성<br>
    - 유지보수가 용이하다.
    - 반복적인 작업을 줄여주기 떄문에 생산성을 높여 준다.
    - 재사용이 가능 하려면 해당 모듈의 의존성이 없어야 한다.
1. 든든한 지원군  
메타(구 페이스북)에서 오픈소스 프로젝트로 관리하고 있어 계속 발전하고 있다.
1. 활발한 지식 공유 &커뮤니티
1. 모바일 앱 개발기능<br>
리액트 네이티브라는 모바일 환경 UI프레임워크를 사용하면,<br>
크로스 플랫폼(cross-platform)모바일 앱을 개발할 수 있다.
### 리액트의 단점
1. 방대한 학습량  
    - 그러나 자바스크립트를 공부한 경우 빠르게 학습할 수 있다.
1. 높은 상태 관리 복잡도  
    - state, component life cycle 등의 개념이 있지만 그리 어렵지 않다.

***
***

## 2023.03.09(2주차)
### 자바스크립트란 ?
- HTML과 CSS로만 구성 돼있던 정적인 웹사이트를 동적인 사이트를 만들기 위한 언어
- ES6(ECMAScript6(2017년에 개발)) - 표준 ECMA-262

<b>자바스크립트의 자료형</b><br>
<li>var : 중복 선언 o, 재할당 o<br>
<li>let(변수) : 중복 선언 x, 재할당 o<br>
<li>const(상수) : 중복 선언 x, 재할당 x

<b>Java Script Object Notation</b> : Key와 Key value(키 값)으로 이루어진 JSON스타일의 자료형

### .gitifnore란?  
- 푸쉬 하지 말아야 될 것들을 안 하게 해주는 것, 원하는 소스코드만 공유 가능
<!-- 자바스크립트의 연산자
대입연산자, 산술연산자, 대입+산술연산자, 증감연산자,(postfix방식, prefix방식) -->
<br>
모든 리포지토리엔 README 파일이 있음.<br>
<b>패키지 매니저</b>를 통해 설치 된 애들은 일괄적(분할)으로 업그레이드가 가능하다.<br><br>

HTML : 웹 사이트의 뼈대를 구성하는 태그들  
CSS :  HTML에서 만들어진 뼈대를 꾸며주는 언어, 문서를 표시하는 방법을  
SPA(Single Page Application) : 한 페이지에 어플리케이션을 정해줌<br>

한번만 쓸 함수는 매개변수만 화살표 함수를 사용.  

패키지 = git 프로그램<br>
구글에서 Git을 검색 후 설치<br>
설치 후 확인은 다음 명령으로 확인한다. $ git —version 혹은 $ git -v
