# 2.1 코드 이해하기

```jsx
import React from 'react';
```

: 리액트를 불러와서 사용할 수 있게 함

모듈을 불러와서 사용하는 것은 원래 브라우저에는 없는 기능 → Node.js에서 지원하는 기능!

(참고, Node.js에서는 require라는 구문으로 패키지를 불러옴)

**번들러(Bundler)**

: 파일을 묶듯이 연결함

ex. **웹팩**, Parcel, browserify 

웹팩 : 편의성과 확장성이 뛰어남!

→ 번들러 도구 사용하면 import로 모듈을 불러왔을 때 불러온 모듈을 모두 합쳐서 하나의 파일을 생성! 또 최적화 과정에서 여러 개의 파일로 분리 가능

**로더(Loader)**

웹팩의 로더 기능 : svg 파일이나 css파일을 불러와 사용 가능!

- **css-loader** : css 파일
- **file-loader** : 웹 폰트나 미디어 파일
- **babel-loader** : 자바스크립트 파일을 불러오면서 최신 자바스크립트 문법으로 작성된 코드를 '바벨'이라는 도구를 사용해 ES5문법으로 변경

→ 원래는 직접 설치해야 하나, create-react-app이 작업을 대신 함!


# 2.2 JSX란?

**자바스크립트의 확장 문법**

xml과 매우 비슷

JSX형식으로 작성한 코드 : 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용해 **일반 자바스크립트 형태의 코드로 변환!**

JSX의 JavaScript 변환

```jsx
// JSX
function App() {
	return {
		<div>
			Hello <b>react</b>
		</div>
	);
```

```jsx
// JavaScript
function App() {
	return React.createElement("div", null, "Hello", React.createElement("b", null, "react");
}
```

⇒ 컴포넌트 렌더링할 때마다 매번 React.createElement() 함수 사용? 매우 불편!!!


# 2.3 JSX의 장점

## 2.3.1 보기 쉽고 익숙하다

**(가독성)**

**JSX > JavaScript**

→ HTML 코드를 작성하는 것과 비슷하기 때문에 JSX를 많이 사용!

→ JavaScript 요소를 일일히 만든다면 불편!

## 2.3.2 더욱 높은 활용도

**JSX에서 HTML 태그 & 컴포넌트도 작성 가능**

`ReactDOM.render`

- 컴포넌트 렌더링 역할
- react-dom 모듈을 불러와 사용
- 첫번째 파라미터: 페이지에 렌더링할 내용을 JSX 형태로 작성
- 두번째 파라미터: 해당 JSX를 렌더링할 document 내부 요소 설정

`<React.StrictMode>`

- 리액트 레걷시 기능들을 사용하지 못하게 하는 기능
- 완전히 사라지게될 옛 기능을 사용했을 때 경고 출력



# 2.4 JSX 문법

## 2.4.1 감싸인 요소

: 컴포넌트에 여러 요소가 있다면 반드시 부모 요소 하나로 감싸야 한다

```jsx
import React from 'react'

function App() {
  return (
    <div>
      <h1>Hello, React!</h1>
      <h2>잘 작동하니?</h2> 
    </div>
  );
}

export default App;
```

- 여래 개의 요소는 **하나의 부모**에 의해 감싸야 한다
- Virtual DOM에서 컴포넌트 **변화를 감지해낼 때 효율적으로 비교**하기 위해 **컴포넌트 내부 역시 하나의 트리 구조로 이루어져야 하는 규칙!**
- `<Fragment></Fragment>` `<></>`

## 2.4.2 자바스크립트 표현

### JSX

- DOM 요소를 렌더링하는 기능
- **JSX 내부에 자바스크립트 표현식 사용 가능 : 코드를 {}로 감싼다**

```jsx
import React from 'react'

function App() {
  const name = '리액트'

  return (
    <div>
      <h1>{name} 안녕!</h1>
      <h2>잘 작동하니?</h2> 
    </div>
  );
}

export default App;
```

(노트)

const : es6에서 새로 도입, 변경 불가능한 상수 선언하는 키워드

let : 동적인 값을 담는 변수

es6 이전에는 var 키워드 사용 : scope(해당값을 사용하는 코드 영역)가 함수단위

→ const, let으로 극복! scope가 블록단위

(주의) const, let은 같은 블록 내부에서 중복 선언 불가능

## 2.4.3 IF문 대신 조건 연산자

JSX 내부의 자바스크립트 표현식에서 if문 사용 불가능

**→ 조건에 따른 렌더링이 필요한 경우, JSX 밖에서 if문을 사용하여 사전에 값을 설정 / { } 안에 조건부 연산자를 사용!**

! 조건부 연산자 == 삼항 연산자

```jsx
import React from 'react'

function App() {
  const name = '리액트'

  return (
    <div>
      {name === '리액트' ? (
        <h1>리액트입니다.</h1>
      ) : (
        <h2>리액트가 아닙니다.</h2>
      )}
    </div>
  );
}

export default App;
```

## 2.4.4 AND 연산자(&&)를 사용한 조건부 렌더링

특정 조건을 만족할 때 내용을 보여주고, 만족하지 않을 때 아무것도 렌더링하지 않아야 하는 상황

- 조건부 연산자를 통해 구현 가능
- AND 연산자 이용시 더 짧게도 가능

```jsx
import React from 'react'

function App() {
  const name = '리액트'

  return (
    <div>
      {name === '리액트' && <h1>리액트입니다.</h1>}
    </div>
  );
}

export default App;
```

(참고)

동등비교 연산자 

=== : 타입과 값 모두 같아야 True 반환

vs.

== : 강제 형변환 수행

## 2.4.5 undefined를 렌더링하지 않기

**리액트 컴포넌트에서는** 함수에서 ndefined만 반환하여 렌더링하는 상황을 만들면 안됨!

**⇒ 어떤 값이 undefined일 수 있다면 OR(||) 연산자를 통해 undefined일 때의 값을 지정해 오류 방지**

```jsx
import React from 'react'
import './App.css'

function App() {
  const name = '리액트'

  return (name || '값이 undefined입니다.');
}

export default App;
```

반면, JSX내부에서 undefined를 렌더링하는 것은 괜찮음!

ex. name 값이 undefined일때 보여주고 싶은 문구가 있는 경우

```jsx
import React from 'react'
import './App.css'

function App() {
  const name = undefined

  return <div>{name || '얍'}</div>;
}

export default App;
```

## 2.4.6 인라인 스타일링

리액트에서 DOM 요소에 **스타일 적용할 때 객체 형태로 넣어**주어야 함!

스타일 이름은 **카멜 표기법(camelCase)**으로 작성해야 함

ex. background-color ⇒ backgroundColor

```jsx
import React from 'react'
import './App.css'

function App() {
  const name = "리액트"

  const style = {
    // camalCase
    backgroundColor : 'black',
    color : 'aqua',
    fontSize : '48px',
    fontWeight : 'bold', 
    padding : 16
  }

  return <div style={style}>{name}</div>;
}

export default App;
```

## 2.4.7 class 대신 className

일반 HTML : css 사용시 class 라는 속성을 설정

ex. <div class="myclass"></div>

**JSX : class가 아닌 className으로 설정**

src/App.css

```css
.react {
  background: aqua;
  color: black;
  font-size: 48px;
  font-weight: bold;
  padding: 16px;
}
```

src/App.js

```jsx
import React from 'react'
import './App.css'

function App() {
  const name = "리액트"

  return <div className="react">{name}</div>;
}

export default App;
```

## 2.4.8 꼭 닫아줘야 하는 태그

JSX에서는 태그를 닫지 않으면 오류 발생!

```jsx
<input></input>
<input /> // self-closing : 태그 사이에 별 내용이 들어가지 않는 경우
```

HTML에서는 <input>, <br>은 열기만하고 닫지 않았음

## 2.4.9 주석

```jsx
{*/ 주석은 이렇게 작성한다 */}
// 1번 주석과
/* 2번 주석은 페이지에 그대로 나타남*/

```


# 2.5 ESLint와 Prettier 적용하기

## 2.5.1 ESLint

문법 검사 도구

코드 실수시 경고 메세지를 vsCode 에디터에서 바로 확인

문제 탭 열기 : 상단메뉴 - 보기 > 문제

초록 줄 코드 : 무시 가능

빨간 줄 코드 : 반드시 고칠 것

## 2.5.2 Prettier

가독성, 코드 정리

*Beautify와 충돌 주의


# 2.6 정리

JSX는 HTML과 비슷하지만 완전히 동일하진 않다!

코드는 XML 형식이지만, 실제로는 JavaScript 객체이며, 용도와 문법도 조금씩 다르다!

2.4 절의 문법을 잘 읽혀 둘 것!
