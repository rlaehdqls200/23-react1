# 201930403 김동빈

## 2023.04.06(6주차)
### 컴포넌트 추출<br>
1. 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 나눌 수도 있다.
1. 큰 컴포넌트에서 일부를 추출해서 새로운 컴포넌트를 만드는 것

```JavaScript
funtion Comment(props) {// Comment는 댓글 표시 컴포넌트.
    return( //내부에는 이미지, 이름, 댓글과 작성일이 포함되어 있다.
        <div className="comment">
            <div className="user-info">
                <img className="avatar"
                    src={props.author.avatarUrl}
                    alt={props.author.name}
                    />
                    <div className="user-info-name">
                        {props.author.name}
                        </div>
                    </div>

                    <div className="comment-text">
                        {props.text}
                    </div>

                    <div className="comment-date">
                        {formatDate(props.date)}
                    </div>
                </div>
    );
}
```

첫 번째로 이미지 부분을 Avatar 컴포넌트로 추출

```JavaScript
funtion Avatar(props) {
    return(
            <img className="avatar"
                src={props.author.avatarUrl}
                alt={props.author.name}
             />  
    );
}
```

추출 후 다시 결합한 UserInfo를 Comment컴포넌트 반영하면, 
다음과 같은 모습이 된다.

```JavaScript
    funtion Comment(props) {
        return(
            <div className="comment">
                <UserInfo user={props.author} />
                <div className="comment-text">
                    {props.text}
                </div>
                <div className="comment-date">
                    {formatDate(props.date)}
                </div>
            </div>
        );
    }
```

처음에 비해서 가독성이 높아진 것을 확인할 수 있다.<br>
* 기본적으로는 한 컴포넌트에 하나의 기능을 수행하도록 설계하는 것이 바람직하다.

그 다음은 사용자 정보 부분을 추출한다.<br>
React컴포넌트 이름은 Camel notatio을 사용한다.<br>


```JavaScript
  funtion UserInfo(props) {//컴포넌트 이름은 UserInfo로 한다.
    return(//UserInfo 안에 Avatar 컴포넌트를 넣어서 완성시킨다.
         <div className="user-info">
            <Avatar user={props.user} />
                <div className="user-info-name">
                    {props.user.name}
                </div>
            </div>
        );
    }
```

이번에는 Comment를 범용으로 사용할 수 있도록 이름과 코멘트를 props로 받도록 수정.<br>

```JavaScript
    function Comment(props){
        return(
        <div style={styles.wrapper}>
            <div style={styles.imageContainer}>
                <img
                    src="https://upload.wikimedia.org/wikipedia/commons/8/89/Portrait_Placeholder.png"
                    alt="프로필 이미지"
                    style={styles.image}
                />
            </div>
            <div style={styles.contentContainer}>
                <span style={styles.nameText}>{props.name}</span>
                <span style={styles.commentText}>{props.comment}</span>
            </div>
        </div>
        )
    }
```

하지만 props로 전달 받은 것이 아직 없기 때문에 지금까지 한걸로는 아무것도 출력되지 않는다.<br>
CommentList를 이용해서 Comment에 props를 전달해보자

```JavaScript
    const comments = [
        {
            name: "김동빈",
            comment: "안녕하세요, 김동빈 입니다."
        },
        {
            name: "김동빈1",
            comment: "안녕하세요, 김동빈1 입니다."
        },
        {
            name: "김동빈2",
            comment: "안녕하세요, 김동빈2 입니다."
        },
    ]

    function CommentList(props){
        return(
            <div>
            {comments.map((foo) => {
                return (
                    <Comment name={foo.name} comment={foo.comment} />
                )
            })}
            </div>
        )
    }
```
별도의 객체로 받아 컴포넌트에서는 이것을 분리하여 출력하도록 해야한다.<br>
이때 사용하는 함수가 map()함수이다.<br>


### State란?
1. State는 리액트 컴포넌트의 상태를 의미한다.<br>
1. 상태의 의미는 정상인지 비정상인지가 아니라 <b>컴포넌트의 데이터</b>를 의미한다.<br>
1. 정확히는 컴포넌트의 <b>변경가능한 데이터</b>를 의미한다.<br>
1. State가 변하면 다시 렌더링이 되기 떄문에 렌더링과 관련된 값만 state에 포함시켜야 한다.

### state의 특징
- 리액트 만의 특별한 형태가 아닌 단지 <b>자바스크립트 객체</b>일 뿐이다.<br>

```JavaScript
    class LikeButton extends React.Component {
        //LikeButton은 class 컴포넌트이다.
        constrctor(props) {//constructor는 생성자
            super(props);

            this.state = { //this.state가 현 컴포넌트의 state이다.
//리액트에 있는 컴포넌트에 state인지 내 state인지 모르기 때문에 this를 붙인다.
                liked: false
            };
        }

        ...
    }
```

- 함수형 에서는 useState()라는 함수를 사용한다.<br>
- state는 변경은 가능하다고 했지만 직접 수정해서는 안된다.<br>
    - 불가능 하다고 생각하는 것이 좋다.<br>

- state를 변경하고자 할 때는 setstate()함수를 사용한다.

```JavaScript
    // state를 직접 수정(잘못된 사용법)
    this.state = {
        name : 'Inje'
    };
    // setState 함수를 통한 수정(정상적인 사용법)
    this.setState({
        name: 'Injs'
    });
```
### 개념 익히기

|용어|설명|
|------|---|
|element|재료|
|component|빵틀|
|instance|재료를 빵틀에 넣고 만든 빵|

### 생명주기에 대해 알아보기
1. 생명주기는 컴포넌트의 생성 시점, 사용 시점, 종료 시점을 나타내는 것.
1. constructor가 실행 되면서 컴포넌트가 생성된다.
1. 생성 직후 componentDidMount() 함수가 호출된다.
1. 컴포넌트가 소멸하기 전까지 여러 번 렌더링 한다.
1. 렌더링은 props, setState(), forceUpdate()에 의해 상태가 변경되면 이루어진다.
1. 그리고 렌더링이 끝나면 componentDinUpdate() 함수가 호출된다.
1. 마지막으로 컴포넌트가 언마운트 되면 compomentWillUnmount()함수가 호출된다.

***
***

## 2023.03.30(5주차)
### 엘리먼트에 대해 알아보기<br>
### 엘리먼트의 정의
- 엘리먼트는 리액트 앱을 구성하는 요소를 의미
- 공식페이지에는 "엘리먼트는 리액트 앱의 가장 작은 빌딩 블록들"이라고 설명하고 있다.
- 웹사이트의 경우는 DOM 엘리먼트이며 HTML요소를 의미함<br>
    <b>리액트 엘리먼트와 DOM엘리먼트의 차이</b><br>
    - 리액트 엘리먼트는 Virtuarl DOM 의 형태를 취하고 있다.
    - DOM 엘리먼트는 페이지의 모든 정보를 갖고 있어 무겁다.
    - 반면 리액트 엘리먼트는 변화한 부분만 갖고 있어 가볍다.<br>

||DOM|Virtual DOM|
|------|---|---|
|업데이트 속도|느림|빠름|
|element 업데이트 방식|DOM 전체를 업데이트| 변화 부분을 가상DOM으로 만든 후<br> DOM과 비교하여 다른 부분만 업데이트|
|메모리| 낭비가 심함| 효율적|
<br>

### 엘리먼트의 생김새(트리구조)<br>
- 리액트 엘리먼트는 자바스크립트 객체의 형태로 존재
- 컴포넌트(Button 등), 속성(color 등)및 내부의 모든 children을 포함하는 일반 JS객체이다.
- 이 객체는 마음대로 변경할 수 없는 불변성을 갖고 있다.

### 엘리먼트의 특징<br>
- 리액트 엘리먼트의 가장 큰 특징은 불변성이다.<br>
    즉, 한 번 생성된 엘리먼트의 children이나 속성(attributes)을 바꿀 수 없다.<Br>
    <b>만일 내용이 바뀌면?</b>
    - 이 떄는 컴포넌트를 통해 새로운 엘리먼트를 생성하면 된다.
    - 그 다음 이전 엘리먼트와 교체를 하는 방법으로 내용을 바꾸는 것
    - 이렇게 교체하는 작업을 하기위해 Virtual DOM을 사용한다.

### 컴포넌트에 대해 알아보기<br>
- 앞서 설명한(3주차) 바와 같이 리액트는 컴포넌트 기반의 구조<br>
    - 컴포넌트 구조라는 것은 작은 컴포넌트가 모여 큰 컴포넌트를 구성하고,<br> 
    이런 컴포넌트들이 모여서 전체 페이지를 구성한다는 것을 의미한다.<br>
- 재사용이 가능하기 떄문에 <u>전체 코드의 양</u>을 줄일 수 있어서 <b>개발 시간과 유지 보수 비용</b>을 줄일 수 있다.<br>
- 엘리먼트를 필요한 만큼 만들어 사용한다는 면에서는 객체 지향의 개념과 비슷하다.
- 컴포넌트는 자바스크립트 함수와 입력과 출력이 있다는 면에서 유사하다.<br>
    - 다만 입력과 출력은 입력은 Props가, 출력은 리액트 엘리먼트의 형태로 출력된다.
    

### 컴포넌트의 종류<br>
리액트 초기 버전을 사용할 떄는 <b>클래스형 컴포넌트</b>를 주로 사용
- Welcome컴포넌트는 React.Component class로 부터 상속을 받아 선언한다.
```JavaScript
 class Welcome extends React.Component {
        render(){
            return <h1>안녕, {this.props.name}</h1>;
        }
} 
```
이후 Hook이라는 개념이 나오면서 최근에는 <b>함수형 컴포넌트</b>를 주로 사용.<br>
Welcome컴포넌트는 props를 받아, 받은 props중 name키의 값을 "안녕,"뒤에 넣어 반환한다.<br>
```JavaScript
funtion Welcome(props){
    return <h1>안녕, {props.name}</h1>;
}
```
예전에 작성된 코드나 문서들이 클래스형 컴포넌트를 사용하고 있기 떄문에
클래스형 컴포넌트와 컴포넌트의 생명주기에 관해서도 알아야한다.<br>
### 컴포넌트의 특징<br>
1. 이름은 항상 대문자로 시작한다 왜냐하면, 리액트는 소문자로 시작하는 컴포넌트를 DOM 태그(html tag)로 인식하기 때문.<br> 
1. 컴포넌트 파일 이름과 컴포넌트 이름은 같게 한다.

1. 컴포넌트 합성<br>
    - 컴포넌트 합성은, 여러 개의 컴포넌트를 합쳐서 하나의 컴포넌트를 만드는 것<br>
    리액트에서는 컴포넌트 안에 또 다른 컴포넌트를 사용할 수 있기 떄문에, 복잡한 화면을 여러 개의 컴포넌트로 나누어 구현할 수 있다.

1. 컴포넌트 추출<br>
    - 컴포넌트 추출은, 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 나누는 것<br>
    큰 컴포넌트에서 일부를 추출해서 새로운 컴포넌트를 만드는 것.



### Props란?<br>
- props는 prop(property : 속성, 특성)의 준말이며, 컴포넌트의 속성이다.<br>
- 컴포넌트에 어떤 속성, props를 넣느냐에 따라서 속성이 다른 엘리먼트가 출력된다.<br>
- props는 컴포넌트에 전달 할 다양한 정보를 담고 있는 <b>자바스크립트 객체</b>이다.

### Props의 특징<br>
- 읽기 전용이라 변경할 수 없다.<br>
- 속성이 다른 엘리먼트를 생성하려면 새로운 props를 컴포넌트에 전달하면 된다.<br>
- pure함수는 인수로 받은 정보가 함수 내부에서도 변하지 않는 함수.<br>
- impure함수는 인수로 받은 정보가 내부에서 변하는 함수.<br>

***
***

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
