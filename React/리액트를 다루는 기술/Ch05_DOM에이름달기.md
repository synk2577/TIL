### DOM 요소의 id

`<div **id="my-element"**></div>`

: html에서 **dom 요소에 이름 부여**하는 방식

→ CSS, JavaScript에서 해당 id를 가진 요소를 찾아서 작업 가능

### ref (reference)

: 리액트 프로젝트 내부에서 **DOM에 이름을 다는 방법**



# 5.1 ref는 어떤 상황에서 사용해야 할까?

Q. '어떤' 작업을 할 때 ref를 사용할까?

**A. DOM을 꼭 직접적으로 건드려야 할 때 사용함!**

ex. 순수 JavaScript 및 jQuery로 만든 웹사이트에서 input 검증할 때; 특정 id를 가진 input에 클래스를 설정함

BUT, 리액트에서는 ~~DOM에 접근~~하지 않아도 **state로 구현 가능**

*이 장에서는 클래스형 컴포넌트에서 ref를 사용하는 방법을 알아볼 것임! (함수형 컴포넌트에서의 ref 사용은 Hooks 필요)

## 5.1.1 예제 컴포넌트 생성

(src > ValidationSample.css)

```css
.sucess {
  background-color: lightseagreen;
}

.failure {
  background-color: lightcoral;
}
```

(src > ValidationSample.js)

```jsx
import { findAllByTestId } from '@testing-library/react';
import React, {Component} from 'react';
import './ValidationSample.css';

class ValidationSample extends Component {
    state = {
        password: '',
        clicked: false, 
        validated: false
    }

    // input에서 onChange 이벤트 발생; state의 password 값을 업데이트
    handleChange = (e) => {
        this.setState({
            password: e.target.value
        });
    }

    // button에서 onClick 이벤트 발생; clied 값을 참으로 설정 validated 값을 검증결과로 설정
    handleButtonClick = () => {
        this.setState({
            clicked:true,
            validated: this.state.password ==='0000'
        })
    }

    render() {
        return(
            <div>
                <input 
                    type="password"
                    value={this.state.password}
                    onChange={this.handleChange}
                    className={this.state.clicked ? (this.state.validated ? 'success' : 'failure') : ''} 
                /> 
                <button onClick={this.handleButtonClick}>검증하기</button>
            </div>
        )
    };
}

export default ValidationSample;
```

: input의 className 값은 버튼을 누르기 전에는 비어있는 문자열을 전달하며, 버튼을 누른 후에는 검증 결과에 따라 success값 또는 failure값을 설정 → 이 값에 따라 input 색상이 초록색/빨간색으로 나타남

## 5.1.2 App 컴포넌트에서 예제 컴포넌트 렌더링

(src > App.js)

```jsx
import React, {Component} from 'react';
import ValidationSample from './ValidationSample.js';

class App extends Component {
  render() {
    return (
      <ValidationSample/>
    );
  }
}

export default App;
```

: App 컴포넌트에서 ValidationSample 컴포넌트 렌더링

(추후 App 컴포넌트에서 ref를 사용할 것이기 때문에 미리 클래스형 컴포넌트로 수정!

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/025a8934-b325-4e30-acf3-dcafd01a08e1/_2020-11-13__9.35.53.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/025a8934-b325-4e30-acf3-dcafd01a08e1/_2020-11-13__9.35.53.png)

## 5.1.3 DOM을 꼭 사용해야 하는 상황

앞 예제서는 state를 사용해 필요한 기능을 구현했지만, state만으로 해결할 수 없는 기능도 존재

- **특정 input에 포커스 주기**
- **스크롤 박스 조작하기**
- **Canvas 요소에 그림 그리기**

⇒ 이러한 상황에서 **ref를 사용해 DOM에 직접 접근!!**



# 5.2 ref 사용

ref를 사용하는 2가지 방법

1. **콜백함수**를 통한 ref 설정
2. **createRef**를 통한 ref 설정

## 5.2.1 콜백 함수를 통한 ref 설정

: ref를 달고자하는 요소에 ref라는 콜백함수를 props로 전달 

→ 이 콜백함수는 ref값을 파라미터로 전달 받음

→ 함수 내부에서 파라미터로 받은 ref를 컴포넌트의 멤버변수로 설정

(콜백함수 사용 예시)

`<input ref={**(ref) => {this.input=ref}**} />`

- `this.input`은 앞으로 input 요소의 DOM을 가르킴
- ref 이름은 원하는 것으로 자유롭게 지정
- DOM 타입과 관계없이 `this.superman = ref`처럼 마음대로 지정

## 5.2.2 createRef를 통한 ref 설정

: 리액트에 내장되어 있는 createRef 함수 사용

(src > RefSample.js)

```jsx
import React, {Component} from 'react';

class RefSample extends Component {
    **input = React.createRef(); // 1. 멤버 변수 선언**

    // DOM에 접근하려면 this.input.current를 조회
    handleFocus = () => {
        **this.input.current.focus();**
    }

    **// 2. 해당 멤버 변수를 달고자 하는 요소에 ref porps로 넣기**
    render() {
        return(
            <div>
                <input **ref={this.input}**/>
            </div>
        );
    }
}

export default RefSample;
```

- 멤버변수로 `React.createRef()`를 담음
- 해당 멤버 변수를 ref를 달고자 하는 요소에 ref props로 넣기
- ref를 설정한 DOM에 접근 : `this.input.current` 조회

## 5.2.3 적용

5.1절의 ValidationSample 컴포넌트의 렌더링 결과 → input 요소를 클릭하면 focus되면서 텍스트 커서가 깜빡임!

버튼 클릭시; focus가 버튼으로 넘어가 input요소에 텍스트 커서가 더 이상 보이지 않음

→ 버튼 한 번 눌렀을 때, 포커스가 다시 input으로 넘어가도록 코드 작성!

### 5.2.3.1 input에 ref 달기

(ValidationSample.js의 input요소)

```jsx
<input
	**ref={(ref) => this.input=ref}**
.. />
**// 콜백함수를 사용해 ValidationSample 컴포넌트에 ref 달기**
```

### 5.2.3.2 버튼 onClick 이벤트 코드 수정

(ValidationSample.js의 handleButtonClick메서드)

```jsx
handleButtonClick = () => {
        this.setState({
            clicked:true,
            validated: this.state.password ==='0000'
        })
        **this.input.focus(); // onClick 이벤트 발생; input에 포커스 줌**
    }
```

: 버튼에서 onClick이벤트 발생할 때 input에 포커스 주도록 코드 수정

`this.input`이 컴포넌트 내부의 input 요소를 가르킴

⇒ (결과) 버튼 클릭 후, focus가 input으로 넘어간다!



# 5.3 컴포넌트에 ref 달기

컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 씀

## 5.3.1 사용법

`<MyComponent **ref={(ref) => {this.myComponent=ref}**}/>`

: MyComponent 내부의 메서드 및 멤버 변수에도 접근 가능 ⇒ 즉, 내부의 ref에도 접근

ex. myComponent.handleClick, myComponent.input 등

스크롤 박스가 있는 컴포넌트를 하나 만들고, 스크롤바를 아래로 내리는 작업을 부모 컴포넌트에서 실행해보자!

(실습 과정)

1. ScrollBox 컴포넌트 만들기
2. 컴포넌트에 ref 달기
3. ref를 이용하여 컴포넌트 내부 메서드 호출하기

## 5.3.2 컴포넌트 초기 설정

### 5.3.2.1 컴포넌트 파일 생성

(src > ScrollBox.js)

```jsx
import React, {Component} from 'react';

class ScrollBox extends Component {
    render() {
        const style = {
          border: '1px solid black',
          height: '300px',
          width: '300px',
          overflow: 'auto',
          position: 'relative'
        };
        const innerStyle = {
          width: '100%',
          height: '650px',
          background: 'linear-gradient(white, black)'
        };
        return (
          <div
            style={style}
            ref={ref => {
              this.box = ref;
            }}
          >
            <div style={innerStyle} />
          </div>
        );  
    }
}

export default ScrollBox;
```

### 5.3.2.2 App 컴포넌트에서 스크롤 박스 컴포넌트 렌더링

(src > App.js)

```jsx
import React, {Component} from 'react';
import ScrollBox from './ScrollBox.js';

class App extends Component {
  render() {
    return (
      <ScrollBox/>
    );
  }
}

export default App;
```

(실행 결과)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d8b9d49-4e7f-44e2-aa26-501570ff11be/_2020-11-14__3.22.51.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d8b9d49-4e7f-44e2-aa26-501570ff11be/_2020-11-14__3.22.51.png)

## 5.3.3 컴포넌트에 메서드 생성

컴포넌트에 스크롤바를 맨 아래쪽으로 내리는 메서드를 만들기

JavaScript로 스크롤바를 내릴때는 DOM 노드가 가진 다음 값들을 활용

- scrollTop : 세로 스크롤바 위치 (0~350)
- scrollHeight : 스크롤이 있는 박스 안의 div 높이 (630)
- clientHeight : 스크롤이 있는 박스의 높이 (300)

⇒ 스크롤바를 맨 아래쪽으로 내리려면 `scrollHeight - clientHeight` !

(src > ScrollBox.js)

```jsx
import React, {Component} from 'react';

class ScrollBox extends Component {

    scorllToBottom = () => {
        **const {scrollHeight, clientHeight} = this.box; // ES6 비구조화 할당문**
        // 아래 코드와 동일
        // const scrollHeight = this.box.scrollHeight;
        // const clientHeight = this.box.clientHeight;
        this.box.scrollTop = scrollHeight - clientHeight;
    }

    render() {
        const style = {
            border: '1px solid black',
            height: '300px',
            width: '300px',
            overflow: 'auto',
            position: 'relative'
        };

        const innerStyle = {
            width: '100%',
            height: '650px',
            background: 'linear-gradient(white, black)'
        };

        return (
          <div
            style={style}
            ref={ref => {
              this.box = ref;
            }}
          >
            <div style={innerStyle} />
          </div>
        );  
    }
}

export default ScrollBox;
```

→ 이 메서드를 부모 컴포넌트인 App 컴포넌트에서 ScrollBox에 ref를 달아 사용해보자!

## 5.3.4 컴포넌트에 ref 달고 내부 메서드 사용

App (부모)컴포넌트에서 ScrollBox에 ref를 달고 버튼을 누르면 

→ ScrollBox 컴포넌트의 scrollToBottom 메서드를 실행하도록 함!

(src > App.js)

```jsx
import React, {Component} from 'react';
import ScrollBox from './ScrollBox.js';

class App extends Component {
  render() {
    return (
      **<div>
        <ScrollBox ref={(ref) => this.scrollBox=ref}/>
        <button onClick={() => this.scrollBox.scorllToBottom()}>맨밑으로</button>
      </div>**
    );
  }
}

export default App;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d34645d1-afeb-40f5-9de7-89fdba7d5795/_2020-11-14__3.38.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d34645d1-afeb-40f5-9de7-89fdba7d5795/_2020-11-14__3.38.29.png)

 (주의)

`onClick = {this.scrollBox.scrollBottom}` 으로 작성해도 문법상 틀린 것은 아님

하지만, 처음 컴포넌트 렌더링시 `this.scrollBox`값이 undefined이므로 `this.scrollBox.SscrollBottom` 값을 읽어오는 과정에서 오류 발생

따라서, 화살표 함수 문법을 사용해 아예 새로운 함수를 만들고 그 내부에서 `this.scrollBox.scrollToBottom`메서드 실행하면 오류 발생하지 않음



# 5.4 정리

컴포넌트 내부에서 DOM에 직접 접근할 때 ref를 사용

(주의) 서로 다른 컴포넌트끼리 데이터 교류할 때 ref를 사용 금지!!!! → 앱 규모 커지면 유지보수 불가능

**컴포넌트끼리 데이터를 교류할 때**는 언제나 데이터를 **부모↔ 자식 흐름**으로 교류해야 함

함수형 컴포넌트에서 ref를 사용하는 방법 → useRef라는 Hook 함수를 사용! (React.createRef와 유사) → 8장에서 자세히..
