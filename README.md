# 201930403 김동빈
## 2023.05.18(12주차)
### 합성
<b>합성(Composition)</b><br>
 '여러 개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것' <br>

- 조합 방법에 따라 합성의 사용 기법은 다음과 같이 나눌 수 있다.
---
### [1] Containment(담다, 포함하다, 격리하다)
1. 특정 컴포넌트가 하위 컴포넌트를 포함하는 형태
2. 컴포넌트에 따라서 어떤 자식 엘리먼트가 들어올 지 예상할 수 없는 경우가 있다.
3. 범용적인 '박스' 역할을 하는 Sidebar 혹은 Dialog와 같은 컴포넌트에서 자주 볼 수 있다.
4. 이런 컴포넌트에서는 children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달하는 것이 좋다.
5. 이때 children prop은 컴포넌트의 props에 기본적으로 들어있는 children속성을 사용한다.
6. 다음과 같이 props.children을 사용하면 해당 컴포넌트의 하위 컴포넌트가 모두 children으로 들어오게 된다.
```JavaScript
    function FancyBorder(props){
        return(
            <div className={'FancyBorder FancyBorder-'+props.color}>
            {props.children}
            </div>
        )
    }
```
- children은 다음 구조에서 세번째 들어가는 파라미터이다.
- 파라미터가 배열로 돼있는 이유는 여러 개의 하위 컾모넌트를 가질 수 있기 때문이다.
- children이 배열로 돼있는 것은 여러 개의 하위 컴포넌트를 위한 것이다.
```JavaScript
    React.createElement(
        type,
        [props],
        [...children]
    )
```
---
### React.createElement()에 관하여
- jsx를 사용하지 않는 경우의 props전달 방법
- 정확히 말하면 JSX를 사용하지 않고 리액트로 엘리먼트를 생성하는 방법.
```JavaScript
    // JSX를 이용한 간단한 방법
    const jsxElement = <h1 className="jsx">JSX Element</h1>
```

- FancyBorder 컴포넌트를 사용하는 예제
- WelconDialog 컴포넌트는 FancyBorder 컴포넌트를 사용하고, FancyBorder 컴포넌트는 h1과 p 두개의 태그를 children이 props로 전달한다.
```JavaScript
   function WelcomeDialog(props){
        return(
            <FancyBorder color="blue">
                <h1 className="Dialog-title">
                    어서오세요
                </h1>
                <p className="Dialog-message">
                    우리 사이트에 방문하신 것을 환영합니다!
                </p>
            </FancyBorder>
        );
    }
```
- 리액트에서는 props.children을 통해 하위 컴포넌트를 하나로 모아서 제공해 준다.
- 만일 여러 개의 children 집합이 필요할 경우는 별도로 props를 정의해서 각각 원하는 컴포넌트를 넣어준다.
- 예와 같이 SpiltPane은 화면을 왼쪽과 오른쪽으로 분할해 주고,<br> App에서는 SpiltPane을 사용해서 left, right 두개의 props를 정의하고 있다.
- 즉, App에서 left, right 를 prop를 받아서 화면을 분할하게 된다.
- 이처럼 여러 개의 children 집합이 필요한 경우 별도의 props를 정의해서 사용한다.
```JavaScript
    function SplitPane(props){
        return(
            <div className="SplitPane">
                <div className="SplitPane-left">
                    {props.left}
                </div>
                <div className="SplitPane-right">
                    {props.right}
                </div>
            </div>
        )
    }

    function App(props) {
        return(
            <SplitPane
                left={
                    <Contacts />
                }
                right={
                    <Chat />
                }
            />
        )
    }
```
---
### [2] SpecialiZation (특수화, 전문화)
- 웰컴다이얼로그는 다이얼로그의 특별한 케이스이다.
- 범용적인 개념을 구별이 되게 구체화하는 것을 특수화라고 한다.
- 객체지향 언어에서는 상속을 사용하여 특수화를 구현한다.
- 리액트에서는 합성을 사용하여 특수화를 구현한다.
- 다음 예와 같이 특수화는 범용적으로 쓸 수 있는 컴포넌트를 만들어 놓고 이를 특수한 목적으로 사용하는 합성 방식이다.
```JavaScript
    funtion Dialog(props) {
        return (
            <FancyBorder color="blue">
            <h1 className = "Dialog-title">
                {props.title}
            </h1>
            <p className = "Dialog-message">
                {props.message}
            </p>
                </FancyBorder>
        );
    }
    funtion WelcomeDialog(props){
        return(
            <Dialog 
                title = "어서 오세요"
                message = "우리 사이트에 방분하신 것을 환영합니다!"
            />
        );
    }
```
---
#### [3] Containment와 Specialization을 같이 사용하기
- Containment를 위해서 props.children을 사용하고, Specialization을 위해 직접 정의한 props를 사용하면 된다.
- Dialog컴포넌트는 이전의 것과 비슷한데 Containment를 위해 끝부분에 props.children을 추가 했다.
- Dialog를 사용하는 SignUpDialog는 Specialization을 위해 props인 title, message에 값을 넣어주고 있고, 입력 받기위해 <.input>과 <.button>을 사용한다 이 두개의 태그는 모두 props.children으로 전달되어 다이얼로그에 표시된다.
- 이러한 형태로 Containment와 Specialization을 동시에 사용할 수 있다.
```JavaScript
    function SignUpDialog(props) {
        const [nickname, setNickname] = useState('');

        const handleChange = (event) => {
            setNickname(event.target.value);
        }

        const handleSignUp = () => {
            alert(`어서오세요, ${nickname}님!`);
        }
        return(
            <Dialog
            title = "화성 탐사 프로그램"
            message = "닉네임을 입력해 주세요.">
            <input
                value={nickname}
                onChange={handleChange}/>
            <button onClick={handleSignUp}>
                가입하기
            </button>
        );
    }
```
### 상속
- 합성과 대비되는 개념으로 상속(inheritance)이 있다.
- 자식 클래스는 부모 클래스가 가진 변수나 함수 등의 속성을 모두 갖게 되는 개념
- 하지만 리액트에서는 상속보다는 합성을 통해 새로운 컴포넌트를 생성한다.<br>

복잡한 컴포넌트를 쪼개 여러개의 컴포넌트로 만들고, 만든 컴포넌트들을 조합하여 새로운 컴포넌트를 만들자

---
### 컨텍스트란 무엇인가?
- 기존의 일반적인 리액트에서는 데이터가 컴포넌트의 props를 통해 부모에게 자식으로 단방향으로 전달되었다.
- 컨텍스트는 리액트 컴포넌트들 사이에서 데이터를 기존의 rpops를 통해 전달하는 방식 대신 '컴포넌트 트리를 통해 곧바로 컴포넌트에 전달하는 새로운 방식'을 제공한다.
- 이 것을 통해 어떤 컴포넌트라도 쉽게 데이터에 접근할 수 있다.
- 컨텍스트를 사용하면 일일이 props로 전달할 필요 없이 그림처럼 데이터를 필요로 하는 컴포넌트에곧바로 데이터를 전달할 수 있다.
---
### 컨텍스트를 사용해야 할 때
- 여러 컴포넌트에서 자주 필요로 하는 데이터는 로그인 여부, 로그인 정보, UI테마, 현재 선택된 언어 등이 있다.
- 또한 반복적인 코드를 계속해서 작성해 주어야 하기 때문에 비효율적이이고 가독성이 떨어진다.
- 컨텍스트를 사용하면 이러한 방식을 깔끔하게 개선할 수 있다.
- 예제에서 처럼 props를 통해 데이터를 전달하는 기존 방식은 실제 데이터를 필요로 하는 컴포넌트까지의 깊이가 깊어질 수록 복잡해 진다.
- React,createContext()함수를 사용해서 ThemeCOntext라는 이름의 컨텍스트를 생성한다.
- 컨텍스트를 사용하려면 컴포넌트의 상위 컴포넌트에서 provider로 감싸주어야 한다.

```JavaScript
// 컨텍스트는 데이터를 매번 컴포넌트를 통해 전달할 필요 없이 컴포넌트 트리로 곧바로 전달하게 해준다.
// 여기에서는 현재 테마를 위한 컨텍스트를 생성하고, 기본값은 'light'이다.
    const ThemeContext = React.createContext('light');

// Provider를 사용하여 하위 컴포넌트들에게 현재 테마 데이터를 전달한다.
// 모든 하위 컴포넌트들은 컴포넌트 트리 하단에 얼마나 깊이 있는지에 관계없이 데이터를 읽을 수 있다.
// 여기에서는 현재 테마값으로 'dark'를 전달한다.
    function App(props) {
        return (
            <ThemeContext.Provider value="dark">
                <Toolbar />
            </ThemeContext.Provider>
        );
    }

// 이젠 중간에 있는 컴포넌트들이 일일이 테마를 하위 컴포넌트로 넘겨줄 필요 없이 필요한 값을 바로 읽을 수 있다.

    function Toolbar(props) {
        return (
            <div>
                <ThemedButton />
            </div>
        );
    }

    function ThemeButton(props) {
    // 리액트는 가장 가까운 상위 테마 Provider를 찾아 그 값을 사용한다.
    // 만약 해당되는 Provider가 없을 경우 기본값을 사용한다.(여기에서는 'light')
    // 여기에서는 상위 Provider가 있기 때문에 현재 테마의 값은 'dark'이다.
        return(
            <ThemeContext.Consumer>
            {value => <Button theme={value} />}
            </ThemeContext.Consumer>
        );
    }
```

- 예제에서는 최상위 컴포넌트인 App컴포넌트에서 Toolbar를 ThemeContext.Probider로 감싸 주었다.
- 이렇게 하면 Provider로 감싼 하위 컴포넌트가 얼마나 깊이 위치해 있는지에 관계없이 컨텍스트의 데이터를 읽을 수 있다.

### 컨텍스트를 사용하기 전에 고려할 점
- 컨텍스트는 다른 레벨의 많은 컴포넌트가 특정 데이터를 필요로 하는 경우에 주로 사용한다.
- 하지만 무조건 컨텍스트를 사용하는 것이 좋은 것은 아니다.
- 왜냐하면 컴포넌트와 컨텍스트가 연동되면 <b>재사용성이 떨어지기 때문</b>
- 따라서 따른 레벨의 많은 컴포넌트가 데이터를 필요로 하는 경우가 아니면 props를 통해 데이터를 전달하는 컴포넌트 합성 방법이 더 적합하다.
- 다음 예제처럼 실제 user와 avatarSize를 사용하는 것은 Avatar컴포넌트 뿐인데 여러단계에 걸쳐 props를 전달하고 있다.
```JavaScript
    // Page 컴포넌트는 PageLayout 컴포넌트를 렌더링
    <Page user={user} avatarSize={avatarSize}/>

    // PageLayout 컴포넌트는 NavigatonBar 컴포넌트를 렌더링
    <PageLayout user={user} avatarSize={avatarSize}/>

    // NavigationBar 컴포넌트는 Link 컴포넌트를 렌더링
    <NavigationBar user={user} avatarSize={avatarSize}/>

    // Link 컴포넌트는 Avatar 컴포넌트를 렌더링
    <Link href={user.permalink}>
    <Avatar user={user} size={avatarSize} />
    </Link>
```
- 이런 경우 컨텍스트를 사용하지 않고 문제를 해결할 수 있는 방법은 Avatar컴포넌트를 변수에 저장하여 직접 넘겨주는 것이다.
- 이렇게 하면 중간 단계의 컴포넌트들은 user와 avatarSize에 대해 몰라도 된다.<br>

아래 예제를 참고하자.
```JavaScript
    function Page(props) {
        const user = props.user;

        const userLink = (
            <Link href ={user.permalink}>
            <Avatar user={user} size={props.avatarSize} />
            </Link>
        );
        // Page 컴포넌트는 PageLayout 컴포넌트를 렌더링
        // 이때 props로 userLink를 전달한다.
        return <PageLayout userLink={userLink} />;
    }

        // PageLayout 컴포넌트는 NavigatonBar 컴포넌트를 렌더링
        <PageLayout userLink={...}/>

        // NavigationBar 컴포넌트는 Link 컴포넌트를 렌더링
        <NavigationBar userLink={...}/>
```
- 하지만 이 예제가 모든 상황에서 좋은 것은 아니다.
- 데이터가 많아질수록 상위 컴포넌트가 점점 더 복잡해지기 때문이다.
- 이런 경우 하위 컴포넌트를 여러 개의 변수로 나눠서 전달하면 된다.<br>

아래의 예제를 참고
```JavaScript
    function Page(props) {
        const user = props.user;

        const topBar = (
            <NavigationBar>
            <Link href={user.permalink}>
                <Avatar user={user} size={props.avatarSize} />
            </Link>
            </NavigationBar>

        );
        const content = <Feed user={user} />;

        return (
            <PageLayout
            topBar={topBar}
            content={content}
            />
        );
    }
```
- 하지만 어떤 경우에는 하나의 데이터에 다양한 레벨에 있는 중첩된 컴포넌트들의 접근이 필요 할 수 있다.
- 이런 경우라면 컨텍스트가 유리하다
- 컨텍스트는 해당 데이터와 데이터의 변경사항을 모두 하위 컴포넌트들에게 broadcast해주기 때문이다.
- 컨텍스트를 사용하기에 적합한 데이터의 대표적인 예로 현재 지역 정보, 테마 그리고 캐싱된 데이터 등이 있다.

### 컨텍스트 API
이 절에서는 리액트에서 제공하는 컨텍스트 API를 통해 컨텍스트를 어떻게 사용하는지에 대해 알아본다.

---
### [1] React.createContext
1. 컨텍스트를 생성하기 위한 함수이다.
2. 파라메다에는 기본값을 넣어주면 된다.
3. 하위 컴포넌트는 가장 가까운 상위 레벨의 Provider로부터 컨텍스트를 받게 되지만,<br> 만일 Provider를 찾을 수 없다면 위에서 설정한 기본값을 사용하게 된다.
---
### [2] Context.Provider
- Context.Provider 컴포넌트로 하위 컴포넌트들을 감싸주면 모든 하위 컴포넌트들이 해당 컨텍스트의 데이터에 접근 할 수 있게 된다.
```JavaScript
    <MyContext.Provider value={/*some vlaue*/}>
```
- Provider 컴포넌트에는 value라는 prop이 있고, 이것은 Provider 컴포넌트 하위에 있는 컴포넌트에게 전달된다.
- 하위 컴포넌트를 consumer 컴포넌트라고 부른다.<br>

[NOTE.Providervalue에서 주의할 사항](#12주차-메모)

---
### [3] Class.contextType
- Provider 하위에 있는 클래스 컴포넌트에서 컨텍스트의 데이터에 접근하기 위해 사용한다.
- Class 컴포넌트는 더 이상 사용하지 않으므로 참고만 한다.
---
### [4] Context.Consumer
함수형 컴포넌트에서 Context.Consumer를 사용하여 컨텍스트를 구독할 수 있다.
```JavaScript
    <MyContext.Consumer>
        {value => /* 컨텍스트의 값에 따라서 컴포넌트들을 렌더링 */}
    </MyContext.Consumer>
```
- 컴포넌트의 자식으로 함수가 올 수가 있는데 이것을 function as a child라고 부른다.
- Context.Consumer로 감싸주면 자식으로 들어간 함수가 현재 컨텍스트의 value를 받아서 리액트 노드로 리턴한다.
- 함수로 전달되는 value는 Provider의 value prop의 값과 같다.
---
### [5] Context.displayName
- 컨텍스트 객체는 displayName이라는 문자열 속성을 갖는다.
-크롬의 리액트 개발자 도구에서는 컨텍스트의 Provider나 Consumer를 표시 할 때 displayName을 함께 표시해 준다.
```JavaScript
    const MyContext = React.createContext(/* some value */);
    MyContext.displayName = 'MyDisplayName';

    //개발자 도구에 "myDisplayName.Provider로 표시됨"
    <MyContext.Provider> 

    // 개발자 도구에 "myDisplayName.Consumer로 표시됨"
    <MyContext.Consumer> 
```
---
### 12주차 메모
> 1. 컨텍스트는 재렌더링 여부를 결정할 때 레퍼런스 정보를 사용하기 때문에 Provider의 부모 컴포넌트가 재렌더링되었을 경우, 의도치 않게 consumer 컴포넌트의 재렌더링이 일어날 수 있다.
예를 들어 아래 코드는 Provider 컴포넌트가 재렌더링될 때마다 모든 하위 consumer 컴포넌트의 재렌더링이 발생한다.<br> 왜냐하면 value prop를 위한 새로운 객체가 매번 새롭게 생성되기 때문.

```JavaScript
    funtion App(props){
        return(
            <MyContext.Provider value={{ something: 'something' }}>
                <Toolbar />
        )
    }
```
---
---
## 2023.05.11(11주차)
### Shared State
1. 공유된 State를 의미한다
1. 하위 컴포넌트가 공통된 부모 컴포넌트의 state를 공유하여 사용하는 것을 Shared State라고 한다.
---

### 하위 컴포넌트에서 State 공유하기
사용자로부터 온도를 입력받아서 해당 온도에서 물이 끓는지 출력해주는 컴포넌트이다.
```JavaScript
    function BoilingVerdict(props) {
        //섭씨온도값을 props로 받는다.
        if (props.celsius >= 100) {
        // 100c 이상이면 끟는다는 문자열을 출력
            return <p>물이 끓습니다.</p>;
        }
        //그 외에는 물이 끓지 않는다는 문자열을 출력한다.
        return <p>물이 끓지 않습니다.</p>;
    }
```
컴포넌트를 실제로 사용하는 부모 컴포넌트
```JavaScript
// 부모 컴포넌트
    function Calculator(props){
        const[temperature,setTemperature]=useState('');
        const handleChange =(event)=>{
            setTemperature(event.target.value);
        }
        return(
            <fieldset>
                <legend>섭씨 온도를 입력하세요:</legend>
                    <input
                    value={temperature}
                    onChange={handleChange}/>
                    <BoilingVerdict
                    celsius={parseFloat(temperature)}/>
            </fieldset>
        )
    }
```
---
### 입력 컴포넌트 추출하기
Calculator 컴포넌트 안에 온도를 입력하는 부분을 별도의 컴포넌트로 추출<br>
* 이렇게 하는 이유는 섭씨온도와 화씨온도를 동시에 따로 입력받을수 있도록 하여<br>
재사용이 가능한 형태로 컴포넌트를 만드는 것이 효율적이기 때문이다.
```JavaScript
//온도를 입력받기 위한 TemperaturerInput 컴포넌트이다.

    const scaleNames = {
        c: '섭씨',
        f: '화씨'
    };

    function TemperatureInput(props) {
        const [temperature, setTemperature] = useState('');

        const handleChange = (event) => {
            setTemperature(event.target.value);
        }

        return (
            <fieldset>
            <legend>온도를 입력해 주세요(단위:{scaleNames[props.scale]}):</legend>
            <input
                value={temperature}
                onChange={handleChange}
            />
            </fieldset>
        )
    }
```

이렇게 추출한 컴포넌트를 사용하도록 Calculator 컴포넌트를 변경하면 아래와 같이 된다.
```JavaScript
    function Calculator(props){
        return(
        /*두개의 입력을 받을 수 있도록 되어있고, 하나는 섭씨온도를 입력받고, 하나는 화씨온도를 입력받는다.*/
            <div>
            <TemperatureInput scale="c"/>
            <TemperatureInput scale="f"/>
            </div>
        )
    }
```
하지만 문제가 발생하게 된다.<br>
사용자가 입력하는 온도값이 TemperatureInput 의 state에 저장되기 때문에 두개의 입력값이 동기화 되지 않는다.

---
### 온도 변환 함수 작성하기
위 [문제가 발생한 컴포넌트](#입력-컴포넌트-추출하기)에서 섭씨온도와 화씨온도 값을 동기화시키기 위해서 각각 변환하는 함수가 필요하다.
```JavaScript
    //화씨 => 섭씨온도로, 섭씨 => 화씨온도로 변환하는 함수이다.
    function toCelsius(fahrenheit) {
        return (fahrenheit - 32) * 5 / 9;
    }

    function toFahrenheit(celcius) {
        return (celsius * 9 / 5) + 32;
    }
```
이렇게 만든 함수를 호출하는 함수를 작성한다
```JavaScript
//온도값과 변환하는 함수를 파라미터로 받아서 값을 변환시켜 리턴해주는 함수이다.
    function tryConvert(temperature, convert) {
        const input = parseFloat(temperature);
            if (Number.isNaN(input)) {
                return '';
            }
        const output = convert(input);
        const rounded = Math.round(output * 1000) / 1000;
            return rounded.toString();
    }
```
만약 숫자가 아닌 값을 입력하면 empty string을 리턴하도록 예외 처리를 한다.
```JavaScript
tryConvert('abc', toCelsius); // 'empty string 을 리턴'
tryConvert('10.22', toFahrenheit); // '50.396을 리턴'
```
---
### Shared State 적용하기
- 다음으로는 하위 컴포넌트의 state를 공통된 부모 컴포넌트로 올려서 shared state를 적용해야한다.
- 여기서 state를 상위 컴포넌트로 올린다는 것을 State 끌어올리기 라고 표현
- 이를 위해 먼저 TemperatureInput 컴포넌트에서 온도 값을 가져오는 부분을 수정해야한다.
```JavaScript
    return(
        // 변경 전:<input value={temperature} onChange={handleChange} />
        <input value={props.temperature} onChange={handleChange} />
    )
```
- 이렇게 하면 온도 값을 컴포넌트의 state에서 가져오는 것이 아닌 props로 가져오게 된다.
- 컴포넌트의 state를 사용하지 않게 되므로 입력값 변경시 상위 컴포넌트로 변경된 값을 전달 해야 한다.
- 이를 위해 handleChange 함수를 수정해준다.
```JavaScript
    const handleChange = (event) => {
        // 변경 전: setTemperature(event.target.value);
        props.onTemperatureChange(event.target.value);
    }
```
- 이제 온도 값을 변경할 때마다 props에 있는<br> onTemperatureChange()함수를 호출하여 상위 컴포넌트로 변경된 값을 전달해준다.
- 최종적인 TemperatureInput 컴포넌트의 코드는 아래와 같다.
```JavaScript
    function TemperatureInput(props){
    const handelChange=(event)=>{
        props.onTemperatureChange(event.target.value);
    }

    return(
      <fieldset>
        <legend>온도를 입력해 주세요(단위:{scaleNames[props.scale]}):</legend>

        <input value={props.temperature} onChange={handleChange} />
      </fieldset>
    )
  }
```
---
### Calculator 컴포넌트 변경하기
마지막으로 변경된 TemperatureInput 컴포넌트를 사용하도록 Calculator 컴포넌트를 변경해야한다.
```JavaScript
    function Calculator(props){
        //state로 temperature와 scale을 선언하여 온도값과 온도의 단위를 저장한다.
        const [temperature,setTemperature] = useState('');
        const [scale,setScale] = useState('c');

        //이 온도와 단위를 이용하여 변환 함수를 통해 섭씨와 화씨온도를 구해서 사용한다.
        const handleCelsiusChange=(temperature) => {
            setScale('c');
            setTemperature(temperature);
        }

        const handleFahrenheitChange=(temperature) => {
            setScale('f');
            setTemperature(temperature);
        }

        const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
        const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

        return(
            <div>
            <TemperatureInput scale = "c" temperature={celsius} onTemperatureChange = {handleCelsiusChange}/>
            <TemperatureInput scale = "f" temperature={fahrenheit} onTemperatureChange = {handleFahrenheitChange}/>
            <BoilingVerdict celcius = {parseFloat(celcius)}/>
            </div>
        )
    }
```
1. TemperatureInput 컴포넌트를 사용하는 부분에서는 각 단위로 변환된 온도값을 props로 넣어주고,<br>
값이 변경되었을때 업데이트하기 위한 함수를 onTemperatureChange로 넣어준다.
1. 따라서 섭씨온도가 변경되면 단위가 'c'로 변경되고, 화씨온도가 변경되면 단위가 'f'로 변경된다.
---
### 11주차 메모
> 1. 모든 컴포넌트에 state가 있는게 아니라 공통된 컴포넌트로 즉, 상위 컴포넌트로 올려서 세팅해버림 
> 1. 같은 맥락에서 사용하는 컴포넌트들이라면 위에서 정의하고 밑에서 내려 받는게 좋다.
> 1. 만약 새로운 state가 필요하면 새로운 인풋을 추가해서 상위 컴포넌트로 보내 <br><b>상위 컴포넌트에서 모두 관리</b>해주는게 좋다.
> 1. 이 방법을 사용하면 리액트에서 더욱 간결하고 효율적인 코드를 작성할 수 있다.
---
---
## 2023.05.04(10주차)
### 리스트와 키
 

|용어|설명|
|------|---|
|리스트|자바스크립트의 변수나 객체를 하나의 변수로 묶어 놓은 배열과 같은 것.|
|키|각 개체나 아이템을 구분할 수 있는 고유한 값을 의미한다.|

- 리액트에서는 배열과 키를 사용하는 반복된 다수의 엘리먼트를 쉽게 랜더링할 수 있다.
- 여러 개의 컴포넌트 랜더링 하기 위해서는 배열에 들어 있는 엘리먼트를 map()함수를 이용하여 랜더링 한다.

```JavaScript
/*numbers 배열에 들어있는 각각의 요소를 map()함수를 이용하여 하나씩 추출하여
2를 곱한 후 doubled라는 배열에 다시 넣는 코드로 일반적인 맵함수이다.*/
    const doubled = numbers.map((number) => number = 2); 
```

다음은 리액트에서 map()함수를 사용한 예제이다.
```JavaScript
   const numbers = [1, 2, 3, 4, 5];
   const listItems = numbers.map((number) =>
   <li>{number}</li>
   );
   //이 코드는 numbers의 요소에 2를 곱하는 대신 <li> 태그를 결합해서 리턴하고 있다.
```

```JavaScript
   ReactDOM.render(
    <ul>
        <li>{1}</li>
        <li>{2}</li>
        <li>{3}</li>
        <li>{4}</li>
        <li>{5}</li>
    </ul>,
    //리턴된 listitems는 <ul> 태그와 결합하여 랜더링 된다. 
    document.getElementById('root')
   );
```

---
### 기본적인 리스트 컴포넌트
- 앞서 작성한 코드를 별도의 컴포넌트로 분리하면 다음과 같다.
- 이 컴포넌트는 props로 받은 숫자를 numbers로 받아 리스트로 랜더링해 준다.
```JavaScript
    function NumberList(props){
        const { numbers } = props;

        const listItems = numbers.map((number) =>
        <li>{number}</li>
        );
        return (
            <ul>{listItems}</ul>
        );
    }

    const numbers = [1, 2, 3, 4, 5];//이거 때문에 key props 오류
    ReactDOM.render(
        <NumberList numbers={numbers} />,
        //맨 윗줄에 props 매개변수에 리턴된다.
        document.getElementById('root')
    );
```
### key props 오류 
이 코드를 실행하면 <b>"리스트 아이템에 무조건 키가 있어야 한다"</b> 는 경고 문구가 나온다.<br>
경고 문구가 나오는 이유는 각각의 아이템에 <b>key props</b>가 없기 때문이다.<br>
[참고용 코드](#attendancebook클래스에-id와-key값-추가)

---
### 리스트의 키에 대해 알아보기
- 리스트에서의 키는 "리스트 에서 아이템을 구별하기 위한 <b>고유한 문자열</b>" 이다.
- 이 키는 리스트에서 어떤 아이템이 변경, 추가 또는 제거되었는지 구분하기 위해 사용한다.
- 키는 <b>같은 리스트에</b> 있는 엘리먼트 사이에서만 <b>고유한 값</b>이면 된다.

---
### 폼이란?
폼은 일반적으로 사용자로부터 입력을 받기 위한 양식에서 많이 사용된다.

### 제어컴포넌트
제어 컴포넌트는 사용자가 입력한 값에 접근하고 제어할 수 있도록 해주는 [컴포넌트](#컴포넌트에-대해-알아보기)이다.

```JavaScript
//사용자의 이름을 입력 받는 HTML폼을 리액트 제어 컴포넌트로 만든 코드
    function NameForm(props){
        const [value, setValue] = useState('');
        //핸들이 두개 있다
        const handleChange = (event) => {
            setValue(event.target.value); //useState에 할당
        }
        const handleSubmit = (event) => {
            alert('입력한 이름: ' + value);
            event.preventDefault();
        }
        return (
             //onChange가 바뀐값을 받고 setValue를 통해 useState에 넣어준다.
            <form onSubmit={handleSubmit}>
                <label>
                    이름:
                <input type="text" value={value} onChange={handleChange}/>
                </label>
                <button type="submit">제출</button>
            </form> 
        )
    }
//State에서 모든걸 관리해주는걸 알 수 있다
```

---
### textarea 태그
HTML에서는 <.textarea>의 children으로 텍스트가 들어가는 형태이다.
```HTML
<textarea>
    안녕하세요, 여기에 이렇게 텍스트가 들어가게 됩니다.
</textarea>
```

리액트에서는 state를 통해 태그의 value라는 attribute를 변경하여 텍스트를 표시한다.
```JavaScript
    function RequestForm(props){
        const [value, setValue] = useState('요청사항을 입력해주세요.');

        const handleChange = (event) => {
            setValue(event.target.value); 
        }
        const handleSubmit = (event) => {
            alert('입력한 요청사항: ' + value);
            event.preventDefault();
        }
        return (
            <form onSubmit={handleSubmit}>
                <label>
                    이름:
                <tetarea value={value} onChange={handleChange} />
                </label>
                <button type="submit">제출</button>
            </form> 
        )
    }
```
---
### select 태그
select태그도 textarea와 동일하다.<br>
```HTML
    <select>
    <option value="apple">사과</option>
    <option value="banana">바나나</option>
    <option selected value="grape">포도</option> //defult값
    <option value="watermelon">수박</option>
    </select>
```

```JavaScript
     function FruitSelect(props){
        const [value, setValue] = useState('grape');//defult값

        const handleChange = (event) => {
            setValue(event.target.value); //useState에 할당
        }
        const handleSubmit = (event) => {
            alert('선택한 과일: ' + value);
            event.preventDefault();
        }
        return (
            <form onSubmit={handleSubmit}>
                <label>
                    과일을 선택하세요:
                    <select value={value} onChange={handleChange}>
                        <option value="apple">사과</option>
                        <option value="banana">바나나</option>
                        <option value="grape">포도</option>
                        <option value="watermelon">수박</option>
                    </select>
                </label>
                <button type="submit">제출</button>
            </form> 
        )
    }
```
---
### File input 태그
 File input 태그는 그 값이 읽기 전용이기 때문에 리액트에서는 비제어 컴포넌트가 된다.<br>

```HTML
    <input type = "file" />
```

---
### Input Null Value
- 제어 컴포넌트에 value prop를 정해진 값으로 넣으면 코드를 수정하지 않는 한 입력값을 바꿀 수 없다.
- 만약 value prop은 넣되 자유롭게 입력할 수 있게 만들고 싶다면 값이 undefind 또는 null을 넣어주면 된다.
```JavaScript
    ReactDOM.render(<input value="hi" />, rootNode);

    setTimeout(function() {
        ReactDOM.render(<input value={null} />, rootNode);
    }, 1000);
```

---
### 10주차 메모

>1. 원래는 컴포넌트를 만들었으면 App.js에서 improt를 해서 가져오기 때문에 index.js는 건들 일이 없다.<br>
*App.js에 컴포넌트가 다 모이기 때문에 수정할건 사실상 App.js밖에 없기 때문
>1. 인덱스 값도 고유한 값이기 때문에 키값으로 사용해도 된다.<br>
*하지만 배열에서 아이템의 순서가 바뀔 수 있는 경우에는 키값으로 인덱스를 사용하는 것을 권장하지 않는다.
>1. 리액트에서는 키를 명시적으로 넣어 주지 않으면 기본적으로 인덱스 값을 키값으로 사용한다. [참조 코드](#index로-키값을-사용하는-경우)
>1. CH10에서 AttendanceBook클래스에 리스트엔 대괄호[] 사용, 아이템은 중괄호{}를 사용, 여러개 사용시 콤마, 로 구분한다
>1. [Key props오류](#key-props-오류) CH10에 AttendanceBook클래스에 key props오류를 고치기 위해 다음과 같이 코드에 id와 key값을 준다.
#### AttendanceBook클래스에 Id와 Key값 추가
```JavaScript
        //students 리스트 내부 아이템에 id 추가
        const students = [
            {
            id: 1, //id 추가
            name: "Inje",
            },
            ...
        ];
        ----------------------------------------------
        <ul>
            {students.map((student) => {
                return <li>{student.name}</li>;
            })}
        </ul>
        //위 코드를 다음과 같이 수정해 key값을 추가한다.
        <ul>
        {students.map((student) => {
                return <li key ={student.id}> {student.name}</li>;
            })}
        </ul>
```
#### index로 키값을 사용하는 경우
```JavaScript
const todoItems = todos.map((todo, index) =>
//아이템들의 고유한 ID가 없을 경우에만 사용해야 함
<li key={ndex}>
    {todo.text}
</li>
//하지만 이 같은 방식은 좋은 방식이 아니기 떄문에 사용하지 않는게 좋음
);
```
---
---

## 2023.04.27(9주차)
### 이벤트 핸들링
> 이벤트가 발생하면 특별한 함수를 실행함으로써 이벤트를 구현시키고 그 함수를 핸들러라 한다.<br>

DOM에서 클릭 이벤트를 처리하는 예제 코드.
```JavaScript
    <button onclick="activate()">
        Activate
    </button>
```

React에서 클릭 이벤트 처리하는 예제 코드.
```JavaScript
    <button onClick={activate}> //camalCase
        Activate
    </button>
```

둘의 차이점은<br>
1. 이벤트 이름이 onclick에서 onClick으로 변경.(Camel case)
1. 전달하려는 함수는 문자열에서 함수 그대로 전달.

이벤트가 발생했을때 해당 이벤트를 처리하는 함수를 <b>"이벤트 핸들러(Event Handler)"</b> 라고 한다.<br>
또는 이벤트가 발생하는 것을 계속 듣고 있다는 의미로 <b>"이벤트 리스너(Event Listener)"</b>라고 부르기도 한다.<br>

이벤트 핸들러 추가하는 방법은?
```JavaScript
    class Toggle extends React.Component{
        constructor(props){
            super(props);

            this.state = { isToggleOn: true };
//callback에서 'this'를 사용하기 위해서는 바인딩을 필수적으로 해줘야 한다.
//bind를 사용하지 않으면 this.handleClick은 글로벌 스코프에서 호출되어, undefined으로 사용할 수 없기 때문이다.
//bind를 사용하지 않으려면 화살표 함수를 사용하는 방법도 있다.
            this.handleClick = this.handleClick.bind(this);
        }

        handleClick(){
            this.setState(prevState => ({
                isToggleOn: !prevState.isToggleOn
            }));
        }

        render(){
            return(
//버튼을 클릭하면 이벤트 핸들러 함수인 handleClick()함수를 호출하도록 되어있다.
                <button onClick = {this.handleClick}>
                    {this.state.isToggleOn ? '켜짐' : '꺼짐'}
                </button>
            );
        }
    }
```
하지만 클래스 컴포넌트는 이제 거의 사용하지 않기 때문에 이 내용은 참고만 한다.<br>

클래스형을 함수형으로 바꾸면 다음 코드와 같다.<br>
함수형에서 이벤트 핸들러를 정의하는 방법은 두 가지가 있다.<br>
함수형에서는 this를 사용하지 않고, onClick에서 바로 HandleClick을 넘기면 된다.<br>
```JavaScript
    funcion Toggle(props){
        const [isToggleOn, setIsToggleOn] = useState(true);
        // 방법 1. 함수 안에 함수로 정의
        funtion handleClick(){
            setIsToggleOn((isToggleOn) => !isToggleOn);;
        }

        // 방법 2. arrow funtion을 사용하여 정의
        const handleClick = () => {
            setIsToggleOn((isToggleOn) => !isToggleOn)
        }
        
        return (
            <button onClick={handleClick}>
                {isToggleOn ? "켜짐" : "꺼짐"} //사망연산자
            </button>
        )
    }
```
---
### Arguments 전달하기
- 함수를 정의할 때는 파라미터(Parameter) 혹은 매개변수, 함수를 사용할 때는 아귀먼트(Argument) 혹은 인수라고 부른다.
- [이벤트 핸들러](#이벤트-핸들링)에 매개변수를 전달해야 하는 경우도 많다.
```JavaScript
    01 <button onClick={(event) => this.deleteItem(id, event)}>삭제하기</button>
    02 <button onClick={this.deleteItem.bind(this,id)}>삭제하기</button>
```
- 위의 코드는 모두 동일한 역할을 하지만 하나는 화살표 함수를, 다른 하나는 bind를 사용했다.
- event라는 매개변수는 <b>리액트의 이벤트 객체</b>를 의미 한다.
- 두 방법 모두 첫 번째 매개변수는 id이고 두 번째 매개변수로 event가 전달된다.
- 첫 번째 코드는 명시적으로 event를 매개변수로 넣어 주었고, <br>두 번째 코드는 id 이후 두번째 매개변수로 event가 자동 전달 된다.
    - 이 방법은 클래스형에서 사용하는 방법이다.

함수형 컴포넌트에서 이벤트 핸들러에 매개변수를 전달할 때는 다음과 같이 한다.
```JavaScript
    function MyButton(props) {
        const handleDelete = (id, event) => {
            console.log(id, event.target);
        }; 
        
        return (
            <button onClick={(event) => handleDelete(1,event)}>삭제하기</button>
        );
    }
```
---
### 조건부 렌더링이란?
여기서 <u>조건이란 우리가 알고 있는 조건문의 조건</u>이다.
```JavaScript
    function Greeting(props) {
        const isLoggendIn = props.isLoggendIn;
        if (isLoggedIn) {
            return <UserGreeting />;
        }
        return <GuestGreeting />;
    }
```
props로 전달 받은 isLoggedln이 true이면 <UserGreeting />을, false면 <GuestGreeting />을 return한다.<br>
이와 같은 <b>렌더링을 조건부 렌더링</b>이라고 한다.

---

### 엘리먼트 변수
렌더링해야 될 컴포넌트를 변수처럼 사용하는 방법이 엘리먼트 변수이다.<br>
다음 코드처럼 state에 따라 button 변수에 컴포넌트 객체를 저장하여 return문에서 사용하고 있다.
```JavaScript
    let buton;
    if (isLoggedIn) {
        button = <LogoutButton onClick = {handleLogoutClick} />;
    } else {
        button = <LoginButton onClick = {handleLoginClick} />;
    }

    return (
        <div>
            <Greeting isLoggedIn = {isLoggedIn} />
            {butto}
        </div>
    )
```
---

### 인라인 조건
<b>필요한 곳에 조건문을 직접 넣어 사용하는 방법.</b>
1. 인라인 if
    - if문을 직접 사용하지 않고, 동일한 효과를 내기 위해 && 논리 연산자를 사용한다.
    - &&는 and연산자로 모든 조건이 참일때만 참이 된다.
    - 첫 번째 조건이 거짓이면 두번째 조건은 판단할 필요가 없다.<br>
    <code>true && expression -> expression<br>false && expression -> false </code>
    ```JavaScript
        {unreadMessages.length > 0 &&
            <h2>
                현재 {unreadMessages.length}개의 읽지 않은 메시지가 있습니다.
            </h2>
        }
    ```
    - 판단만 하지 않는 것이고 결과 값은 그대로 리턴된다.

1. 인라인 if-else
    - 삼항 연산자를 사용한다. <code>조건문 ? 참일 경우 : 거짓일 경우</code>
    - 문자열이나 엘리먼트를 넣어서 사용할 수도 있다.
    ```JavaScript
        funtion UserStatus(props) {
            return (
                <div>
                이 사용자는 현재 <b>{props.isLoggedIn ? '로그인' : '로그인하지 않은'}
                </b> 상태입니다.
                </div>
            )
        }

        <div>
            <Greeting isLoggedIn = {isLoggedIn} />
            {isLoggedIn
                ? <LogoutButton onClick = {handleLogoutClick} />
                : <LoginButton onClick = {handleLoginClick} />
                }
        </div>
    ```
---
### 컴포넌트 렌더링 막기
컴포넌트를 렌더링하고 싶지 않을때에는 null을 리턴한다.
```JavaScript
    function WarningBanner(props){
        if (!props.warning) {
            return null;
        }

        return (
            <div>경고!</div>
        );
    }
```
---
### 9주차 메모
> 1. React에서는 Pascalcase를 사용하며, 첫 문자를 대문자로 사용한다.<br>
> 1. 문자와 문자사이에 언더바(_)가 들어가는걸스네이크 함수라고 한다.
> 1. 배열은 자료구조가 아닌 스택이나 큐 같은 자료를 구현하기 위한것

---
---

## 2023.04.13(7주차)
### 훅이란 무엇인가?
1. 클래스형 컴포넌트에서는 생성자(constructor)에서 state를 정의하고,<br> setState() 함수를 통해 state를 업데이트 한다.

    |발전 과정|
    |:-:|
    |과거 함수형 컴포넌트는 별도로 state를 정의하거나, <br>컴포넌트의 생명주기에 맞춰서 코드를 실행 할 수 없었다.|
    |▼|
    |그런 함수형 컴포넌트의 단점을 보완하기 위해<br> 추가된 기능이 바로 훅(Hook)이다.
    |▼|
    |함수형 컴포넌트도 훅을 통해<br> 클래스형 컴포넌트의 기능을 모두 동일하게 구현할 수 있게 되었다.|


1. Hook이란 <b>'state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행되도록 만든 함수'</b>를 의미

1. 훅의 이름은 모두 'use'로 시작한다.

1. 사용자 정의 훅(custom hook)을 만들 수 있으며, 이 경우 또한 이름은 'use'로 시작해야 한다.
    - 그렇지 않을경우 wring이나 error가 난다.
---
### useState
ueState는 함수형 컴포넌트에서 state를 사용하기 위한 Hook이다.<br>

다음 예제는 버튼을 클릭할 떄마다 카운트가 증가하는 함수형 컴포넌트 이다.<br>
- 하지만 증가는 시킬 수 있지만 증가할 때마다 재 렌더링은 일어나지 않는다.<br>
- 이럴때 state를 사용해야 하지만 함수형에는 없기 떄문에 useState()를 사용한다.

useState() 함수의 사용법<br>
1. 첫번째 항목은 state의 이름(변수명)이다
1. 두번째 항목은 state의 set함수이다. 즉, state를 업데이트 하는 함수이다.
    - 변수명과 함수명은 동일하게 가져가는게 좋다
1. 함수를 호출 할 때 state의 초기값을 설정한다.
1. 함수의 리턴 값은 배열의 형태이다. 
---
### useEffect
useState와 함께 가장 많이 사용하는 Hook이다.<br>
이 함수는 사이드 이펙트를 수행하기 위한 것이다.<br>
프로그래밍에서 사이트 이펙트는 '개발자가 의도하지 않은 코드가 실행되면서 버그가 발생하는 것'을 말한다.<br>
하지만 리액트에서는 효과 또는 영향을 뜻하는 effect의 의미에 가깝다.<br>
- 예를 들면 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 등의 작용을 의미한다.<br>

이 작업을 이펙트라고 부르는 이유는 이 작업들이 다른 컴포넌트에 영향을 미칠 수 있으며,<br> 렌더링 중에는 작업이 완료될 수 없기 때문이다. <br>
- 렌더링이 끝난 이후에 실행되어야 하는 작업들이다.<br>

클래스 컴포넌트의 생명주기 함수와 같은 기능을 하나로 통합한 기능을 제공한다.<br>
* 저자는 useEffect가 side effect가 아니라 effect에 가깝다고 설명하고 있지만,<br> 이것은 부작용의 의미를 不(아닐,부)로 잘못 해석해서 생긴 오해이다.<br>
* side effect는 으로 '원래의 용도 혹은 목적의 효과 외에 부수적으로 다른 효과가 있는것'을 의미한다.<br>
* 결국 sideEffect는 렌더링 외에 실행해야 하는 부수적인 코드를 말한다.<br>
* 예를 들면 네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리(clan-up)가 필요 없는 경우들이다.<br>
    
useEffect()함수는 다음과 같이 사용한다.<br>
첫 번째 파라미터는 이펙트 함수가 들어가고, 두 번째 파라미터로는 의존성 배열이 들어간다.
```JavaScript
    useEffect(이펙트 함수, 의존성 배열);
```
의존성 배열은 이펙트가 의존하고 있는 배열로,<br> 배열 안에 있는 변수 중에 하나라도 값이 변경되었을 때 이펙트 함수가 실행된다.<br>
이펙트 함수는 처음 컴포넌트가 렌더링 된 이후, 그리고 재 렌더링 이후에 실행된다.<br>
만약 이펙트 함수가 마운트와 언마운트 될 때만 한 번씩 실행되게 하고 싶으면 빈 배열을 넣으면 된다.<br>
이 경우 props나 state에 있는 어떤 값에도 의존하지 않기 때문에 여러 번 실행되지 않는다.
```JavaScript
    useEffect(() => {
        // 컴포넌트가 마운트 된 이후,
        // 의존성 배열에 있는 변수들 중 하나라도 같이 변경되었을 때 실행됨
        // 의존성 배열에 빈 배열([])을 넣으면 마운트와 언마운트시에 단 한 번씩만 실행됨
        // 의존성 배열 생략 시 컴포넌트 업데이트 시마다 실행됨
        ...
        return() => {
             // 컴포넌트가 마운트 해제되기 전에 실행됨
            ...
        }
    }, [의존성 변수1, 의존성 변수2, ...]);
```
---
### useMemo
useMemo() 혹은 Memoizde value를 리턴하는 훅이다.<br>
이전 계산값을 갖고 있기 때문에 연산량이 많은 작업의 반복을 피할 수 있다.<br>
이 혹은 렌더링이 일어나는 동안 실행된다.<br>
따라서 렌더링이 일어나는 동안 실행돼서는 안 될 작업을 넣으면 안된다.<br>
예를 들면 useEffect에서 실행되어야 할 사이드 이펙트 같은 것이다.
```JavaScript
    const memoizedValue = useMemo(
        () => {
            // 연산량이 높은 작업을 수행하여 결과를 반환
            return computeExpensiveValue(의존성 변수1, 의존성 변수2);
        },
        [의존성 변수1, 의존성 변수2]
    );
```
다음 코드와 같이 의존성 배열을 넣지 않을 경우, 렌더링이 일어날 때마다 매번 함수가 실행된다.<br>
따라서 의존성 배열을 넣지 않는 것은 의미가 없다.<br>
만약 빈 배열을 넣게 되면 컴포넌트 마운트 시에만 함수가 실행된다.
```JavaScript
    const memoizedValue = useMemo(
        () => computeExpensiveValue(a, b)
    );
```
---
### useCallback
useCallback() 혹은 useMemo()와 유사한 역할을 위한 훅이다.<br>
차이점은 값이 아닌 함수를 반환한다는 점이다.<br>
의존성 배열을 파라미터로 받는 것은 useMemo와 동일 하다.<br>
파라미터로 받은 함수를 콜백이라고 부른다.<br>
useMemo와 마찬가지로 의존성 배열 중 하나라도 변경되면 콜백함수를 반환한다.
```JavaScript
    const memoizedCallback = useCallback(
        () => {
            doSomething(의존성 변수1, 의존성 변수2);
        },
        [의존성 변수1, 의존성 변수2]
    );
```
---
### useRef
useRef() 혹은 레퍼런스를 사용하기 위한 훅이다.<br>
레퍼런스란 특정 컴포넌트에 접근할 수 있는 객체를 의미한다.<br>
useRef() 혹은 바로 이 레퍼런스 객체를 반환한다.<br>
레퍼넌스 객체에는 .current라는 속성이 있는데, 이것은 현재 참조하고 있는 엘리먼트를 의미한다.
```JavaScript
    const refContainer = useRef(초기값);
```
이렇게 반환된 레퍼런스 객체는 컴포넌트의 라이프타임 전체에 걸쳐서 유지된다.<br>
즉, 컴포넌트가 마운트 해제 전까지는 계속 유지된다는 의미이다.<br>

---
### 훅의 규칙
1. 첫 번째 규칙은 무조건 최상의 레벨에서만 호출해야 한다는 것이다.<br>
여기서 최상위는 컴포넌트의 최상위 레벨을 의미한다.<br>
   - 함수가 실행 될 때 가장 먼저 실행 되어야 한다.<br>

    따라서 반복문이나 조건문 또는 중첩된 함수들 안에서 훅을 호출하면 안된다.<br>
이 규칙에 따라서 훅은 컴포넌트가 렌더링 될 때마다 같은 순서로 호출되어야 한다.<br>
1. 두 번째 규칙은 리액트 함수형 컴포넌트에서만 훅을 호출해야 한다는 것이다.<br>
따라서 일반 자바스크립트 함수에서 훅을 호출하면 안 된다.<br>
훅은 리액트의 함수형 컴포넌트 혹은 직접 만든 커스텀 훅에서만 호출할 수 있다.<br>
    * 훅을 만드는 것은 함수형 컴포넌트를 만드는 것과 동일하다.<br>
---
### 나만의 훅 만들기
필요 하다면 직접 훅을 만들어 쓸 수도 있다,
이것을 커스텀 훅이라고 한다.<br>
1. 커스텀 훅을 만들어야 하는 상황<br>
예제 UserStatus 컴포넌트는<br> isOnline이라는 state에 따라서 사용자의 상태가 온라인인지 아닌지를 텍스트로
보여주는 컴포넌트 이다.
```JavaScript
    function UserStatus(props) { //import랑 Exsport는 생략
        const [isOnline, setIsOnline] = useState(null);

        useEffect(() => {
            function handleStatusChange(status) {
                setIsOnline(status.isOnline);
            }

            ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
            return () => {
                ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
            };
        });
        if (isOnline === null) {
            return '대기중...';
        }
        return isOnline ? '온라인' : '오프라인';
    }
```

```JavaScript
    function UserListItem(props) { //import랑 Exsport는 생략
        const [isOnline, setIsOnline] = useState(null);
            //위에 코드에서 useState와 useEffect 훅을 사용하는 부분이 동일하다.
            // 이렇게 state와 관련된 로직이 중되는 경우에
            // render props 또는 HOC를 사용한다.
        useEffect(() => {
            function handleStatusChange(status) {
                setIsOnline(status.isOnline);
            }

            ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
            return () => {
                ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
            };
        });
    }
```
2. 커스텀 훅 추출하기<br>
두 개의 자바스크립트 함수에서 하나의 로직을 공유하도록 하고 싶을 때,<br> 새로운 함수를 하나 만드는 방법을 사용한다.<br>
리액트 컴포넌트와 혹은 모든 함수이기 때문에 동일한 방법을 사용할 수 있다.<br>
이름은 use로 시작하고, 내부에서 다른 훅을 호출하는 자바스크립트 함수를 만들면 된다.<br>
아래 코드는 중복되는 로직을 useUserStatus()라는 커스텀 훅으로 추출해낸 것.

```JavaScript
    function useUserStatus(userId) { //import랑 Exsport는 생략
        const [isOnline, setIsOnline] = useState(null);
    
        useEffect(() => {
            function handleStatusChange(status) {
                setIsOnline(status.isOnline);
            }

            ServerAPI.subscribeUserStatus(user.id, handleStatusChange);
            return () => {
                ServerAPI.unsubscribeUserStatus(user.id, handleStatusChange);
            };
        });
        return isOnline;
    }
```
일반 컴포넌트와 마찬가지로 다른 훅을 호출하는 것은 무조건 커스텀 훅의 최상위 레벨에서만 해야 한다.<br><br>
커스텀 훅은 일반 함수와 같다고 생각해도 된다.<br>
- 다만 이름은 use로 시작한다는게 차이점.<br><br>

3. 커스텀 훅 사용하기<br>
```JavaScript
    function UserStatus(props) { //import랑 Exsport는 생략
        const isOnline = useUserStatus(props.user.id);
    
            if (isOnline === null) {
                return '대기중...';
            }
            return isOnline ? '온라인' : '오프라인';
        }
            function UserListItem(props) {
                const isOnline = useUserStatus(props.user.id);

                return (
                    <li style={{ color: isOnline ? 'green' : 'black' }}>
                        {props.user.name}
                    </li>
            );
    }
```
가독성이 좋아지고, 생산성도 빨라진다.

***
***

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
