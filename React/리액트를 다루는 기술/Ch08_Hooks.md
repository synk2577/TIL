### Hooks

함수형 컴포넌트에서도 상태관리할 수 있는 useState, 렌더링 직후 작업을 설정하는 useEffect 기능을 제공

→ 함수형 컴포넌트에서의 다양한 작업을 가능하게 함

  
# 8.1 useState

### useState

가장 기본적인 Hook

함수형 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해줌

### useState 기능을 사용한 숫자 카운터

(src > Counter.js)

```jsx
import React, {useState} from 'react'; // useState : import 구문을 통해 불러옴

const Counter = () => {
    const [value, setValue] = useState(0); // useState의 사용

    return(
        <div>
            <p>
                현재 카운터 값은 <b>{value}</b>입니다.
            </p>
            <button onClick={()=>setValue(value+1)}>+1</button>
            <button onClick={()=>setValue(value-1)}>-1</button>
        </div>
    );
};

export default Counter;
```

`const [value, setValue] = useState(0); // useState의 사용`

→ useState 함수의 파라미터 : 상태의 기본 값을 넣어줌

→ 현재 0의 값 넣음 = 카운터의 기본 값을 0으로 설정

useState 함수가 호출되면, 배열 반환

- 첫번째 원소 : 상태값 (value)
- 두번째 원소 : 상태를 설정하는 함수 (setValue)

useState 함수에 파라미터(상태의 기본 값)를 넣어서 호출;

전달받은 파라미터로 값이 바뀌고 컴포넌트가 정상적으로 리렌더링

(src > App.js)

```jsx
import React from 'react';
import Counter from './Counter.js'

const App = () => {
  return <Counter />;
};

export default App;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ba72168-53d5-4af7-9020-dae043723fce/_2020-11-20__11.07.34.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ba72168-53d5-4af7-9020-dae043723fce/_2020-11-20__11.07.34.png)

useState로 카운터 구현하기

## 8.1.1 useState를 여러 번 사용하기

하나의 useState함수는 하나의 상태 값만 관리 가능

컴포넌트에서 관리해야 할 상태가 여러개라면 useState 여러 번 사용

(src > Info.js)

```jsx
import React, {useState} from 'react';

const Info = () => {

    const [name, setName] = useState('');
    const [nickname, setNickname] = useState('');

    const onChangeName = e => {
        setName(e.target.value);
    };

    const onChangeNickname = e => {
        setNickname(e.target.vlaue);
    };
    
    return (
        <div>
            <div>
                <input value={name} onChange={onChangeName} />
                <input value={nickname} onChange={onChangeNickname} />
            </div>
            <div>
                <div>
                    <b>이름: </b> {name}
                </div>
                <div>
                    <b>닉네임: </b> {nickname}
                </div>
            </div>
        </div>
    );
};

export default Info;
```

(src > App.js)

```jsx
import React from 'react';
import Info from './Info.js'

const App = () => {
  return <Info />;
};

export default App;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd5855bd-a567-4bc2-aec1-19e5b30e4034/_2020-11-20__11.30.49.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd5855bd-a567-4bc2-aec1-19e5b30e4034/_2020-11-20__11.30.49.png)

useState 여러 번 사용하기



# 8.2 useEffect

### useEffect

리액트 컴포넌트가 **렌더링될 때마다 특정 작업을 수행**하도록 설정

클래스형 컴포넌트의 componentDidMount와 componentDidUpdate를 합친 상태

### 기존에 만들었던 Info 컴포넌트에 useEffect 적용

```jsx
import React, {useState, useEffect} from 'react';

const Info = () => {

    const [name, setName] = useState('');
    const [nickname, setNickname] = useState('');
    useEffect(() => {
        console.log('렌더링이 완료되었습니다!');
        console.log({
            name,
            nickname
        });
    });

    const onChangeName = e => {
        setName(e.target.value);
    };

    const onChangeNickname = e => {
        setNickname(e.target.vlaue);
    };
    
    return (
        <div>
            <div>
                <input value={name} onChange={onChangeName} />
                <input value={nickname} onChange={onChangeNickname} />
            </div>
            <div>
                <div>
                    <b>이름: </b> {name}
                </div>
                <div>
                    <b>닉네임: </b> {nickname}
                </div>
            </div>
        </div>
    );
};

export default Info;
```

input에 값이 입력될 때마다 console에 log남음

## 8.2.1 마운트될 때만 실행하고 싶을 때

useEffect에서 설정한 함수를 컴포넌트가 화면에 맨 처음 렌더링 될 때만 실행하고 업데이트 될 때는 실행하지 않으려면, **함수의 두번째 인자로 빈 배열**을 넣어야 함!

(src > Info.js - useEffect)

```jsx
useEffect(() => {
        console.log('마운트될 때만 실행됩니다');
    }, **[]**);
```

컴포넌트가 처음 나타날때만 콘솔에 문구가 나타나고, 그 이후에는 나타나지 않음

## 8.2.2 특정 값이 업데이트될 때만 실행하고 싶을 때

useEffect를 사용할 때, 특정 값이 변경될 때만 호출하고 싶은 경우

### 클래스형 컴포넌트의 경우

```jsx
componentDidUpdate(prevProps, prevState) {
	if (prevProps.value !== this.props.value) {
		doSomething();
	}
}
```

props안에 들어있는 value 값이 바뀔 때만 특정 작업을 수행

### useEffect

두번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣으면 됨

(src > Info.js - useEffect)

```jsx
useEffect(() => {
        console.log(name);
    }, [name]);
```

배열 안에는 **useState를 통해 관리하고 있는 상태**를 넣어주어도 되고, **props로 전달받은 값**을 넣어도 됨

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2506ab95-ce6e-42ef-ab86-42a9d6b9d591/_2020-11-20__11.48.07.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2506ab95-ce6e-42ef-ab86-42a9d6b9d591/_2020-11-20__11.48.07.png)

특정 값이 업데이트될 때만 실행하기

## 8.2.3 뒷정리 하기

### useEffect

**렌더링되고 난 직후 실행**

**두 번째 파라미터 배열**에 무엇을 넣는지에 따라 **실행되는 조건이 달라짐** 

### 뒷정리 함수 반환

컴포넌트가 언마운트되기 전이나 업데이트되기 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리 함수(cleanup)를 반환

(src > Info.js - useEffect)

```jsx
useEffect(() => {
        console.log('effect');
        console.log(name);
        return () => {
            console.log('cleanup');
            console.log(name);
        };
    });
```

(src > App.js) - Info 컴포넌트의 가시성을 바꿀수 있게 함

```jsx
import React, {useState} from 'react';
import Info from './Info.js'

const App = () => {

  const [visible, setVisible] = useState(false);
  return (
    <div>
      <button
        onClick={()=> {
          setVisible(!visible);
        }}
      >
        {visible ? '숨기기' : '보이기'}
      </button>
      <hr/>
      {visible && <Info />}
    </div>
  )

};

export default App;
```

보이기/숨기기 버튼 눌러보기

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3be5c437-876d-4d2f-bd7b-d35917d9ca6b/_2020-11-21__12.01.56.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3be5c437-876d-4d2f-bd7b-d35917d9ca6b/_2020-11-21__12.01.56.png)

useEffect 정리

컴포넌트가 **나타날때 콘솔에 effect** 나타남, **컴포넌트가 사라질 때 cleanup**이 나타남

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/51e4030f-d79d-41fa-8499-aa0d85c3b66f/_2020-11-21__12.02.23.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/51e4030f-d79d-41fa-8499-aa0d85c3b66f/_2020-11-21__12.02.23.png)

업데이터할 때마다 발생하는 effect와 cleanup

오직 **언마운트될 때만 뒷정리 함수를 호출**하고 싶다면 

useEffect 함수의 **두 번째 파라미터에 비어있는 배열**을 넣으면 됨!

(src > Info.js - useEffect)

```jsx
useEffect(() => {
        console.log('effect');
        console.log(name);
        return () => {
            console.log('cleanup');
            console.log(name);
        };
    }, **[name]**);
```



# 8.3 useRender

### useRender

useState보다 더 다양한 컴포넌트 상황에 따라 **다양한 상태를 다른 값으로 업데이**트 해주고 싶을 때 사용하는 Hook

### Reducer (리듀서)

현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action)값을 전달받아 **새로운 상태를 반환**하는 함수

reducer 함수에서 새로운 상태를 만들 때는 반드시 **불변성**을 지켜주어야 함

```jsx
function reducer(state, action) { 
	return { ... }; // 불변성을 지키면서 업데이트한 새로운 상태를 반환
```

action값은 주로 다음과 같은 형태

```jsx
{ 
type: 'INCERMENT',
// 다른 값들이 필요하다면 추가로 들어감
}
```

- 17장의 리덕스에서 사용하는 액션 객체 : 어떤 액션인지 알려주는 type필드가 꼭 필요
- **useRender**에서 사용하는 **액션 객체** : **반드시 type을 지니고 있을 필요가 없음**, 심지어 객체가 아니라 문자열/숫자여도 상관 없음

## 8.3.1 카운터 구현하기

useRender를 사용해 기존의 Counter 컴포넌트 다시 구현

(src > Counter.js)

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
  // action.type에 따라 다른 작업 수행
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 };
    case 'DECREMENT':
      return { value: state.value - 1 };
    default:
      // 아무것도 해당되지 않을 때 기존 상태 반환
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 }); // 리듀서함수, 해당 리듀서의 기본 값

  return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value}</b>입니다.
      </p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+1</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-1</button>
    </div>
  );
};

export default Counter;
```

useReducer

- 첫번째 파라미터 : 리듀서 함수
- 두번째 파라미터 : 해당 리듀서의 기본 값

state : 현재 가르키고 있는 상태
dispatch : 액션을 발생시키는 함수 → dispatch(action)과 같은 형태로, 함수 안에 파라미터로 액션 값을 넣어주면 리듀서 함수가 호출되는 구조

### useReducer의 장점

컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있음

(src > App.js)

```jsx
import React from 'react';
import Counter from './Counter'

const App = () => {
  return <Counter />;
};

export default App;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cde9fb9b-cc03-4922-905e-294d13acc01b/_2020-11-21__12.38.42.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cde9fb9b-cc03-4922-905e-294d13acc01b/_2020-11-21__12.38.42.png)

## 8.3.2 인풋 상태 관리하기

useReducer를 이용해 Info 컴포넌트에서 인풋 상태 관리하기

기존에 클래스형 컴포넌트에서 input 태그에 name 값을 할당하고 e.target.name을 참조하여 setState를 해준 것과 유사한 방식으로 작업 처리

(src > Info.js)

```jsx
import React, {useReducer} from 'react';

function reducer(state, action) {
    return {
        ...state,
        [action.name]: action.value
    }
}

const Info = () => {
    const [state, dispatch] = useReducer(reducer, {
        name: '',
        nickname: ''
    });
    const { name, nickname } = state;

    const onChange = e => {
        dispatch(e.target);
    }
    
    return (
        <div>
            <div>
                <input name="name" value={name} onChange={onChange} />
                <input name="nickname" value={nickname} onChange={onChange} />
            </div>
            <div>
                <div>
                    <b>이름: </b> {name}
                </div>
                <div>
                    <b>닉네임: </b> {nickname}
                </div>
            </div>
        </div>
    );
};

export default Info;
```

userRender에서의 action은 그 어떤 값도 사용 가능

이 예제서는 이벤트 객체가 지니고 있는 **e.target 값 자체를 액션 값으로 사용**함

⇒ 이런 식으로 input을 관리하면 인풋의 개수가 많아져도 **코드를 짧고 간단하게 유지** 가능

(src > App.js)

```jsx
import React from 'react';
import Info from './Info'

const App = () => {
  return <Info />;
};

export default App;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a97581f-879f-46ce-be33-5917b58bfd26/_2020-11-21__12.51.57.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a97581f-879f-46ce-be33-5917b58bfd26/_2020-11-21__12.51.57.png)



# 8.4 useMemo

### useMemo

함수형 컴포넌트 내부에서 발생하는 연산 최적화

리스트에 숫자 추가하면 추가된 숫자들의 평균을 보여주는 함수형 컴포넌트

(src > Average.js)

```jsx
import React, { useState } from 'react';

const getAverage = numbers => {
  console.log('평균값 계산 중..');
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const onChange = e => {
    setNumber(e.target.value);
  }; 
  const onInsert = e => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
  }; 

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {getAverage(list)}
      </div>
    </div>
  );
};

export default Average;
```

(src > App.js)

```jsx
import React from 'react';
import Average from './Average'

const App = () => {
  return <Average />;
};

export default App;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/baabf184-bbd8-4e2b-9c81-8dbc422e6b51/_2020-11-21__1.14.51.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/baabf184-bbd8-4e2b-9c81-8dbc422e6b51/_2020-11-21__1.14.51.png)

숫자는 3개 입력되어 있지만 로그는 3개 이상임 → 숫자를 등록할 때 뿐만 아니라 input의 내용이 수정될 때도 getAverage함수가 호출되는 것을 의미

input 내용이 바뀔 때는 평균값을 다시 계산할 필요가 없는데 계산 되는 것은 **낭비임!!!**

이때, `**useMemo**` Hook을 사용하면 최적화 가능

⇒ 렌더링하는 과정에서 **특정 값이 바뀌었을 때만 연산을 실행**하고, **원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용**하는 방식

(src > Average.js)

```jsx
import React, { useState, useMemo } from 'react';

const getAverage = numbers => {
  console.log('평균값 계산 중..');
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const onChange = e => {
    setNumber(e.target.value);
  }; 
  const onInsert = e => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
  }; 

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/acf66501-7821-42c1-afda-e211c0d42983/_2020-11-21__1.19.41.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/acf66501-7821-42c1-afda-e211c0d42983/_2020-11-21__1.19.41.png)

이제 배열이 바뀔 때만 getAverage 함수가 호출됨!!



# 8.5 useCallback

### useCallback

주로 렌더링 성능을 최적화 하는 상황에서 사용

만들어 놨던 함수 재사용 가능

좀 전에 구현한 Average 컴포넌트에서 onChange, onInsert 함수를 선언

→ 컴포넌트가 리렌더링 될 때마다 새로 만들어진 함수를 사용

→ **컴포넌트 렌더링이 자주 발생** or **리렌더링할 컴포넌트의 개수가 많아진**다면 최적화가 필요해짐

(src > Average.js)

```jsx
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = numbers => {
  console.log('평균값 계산 중..');
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const onChange = useCallback(e => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링 될 때만 함수 생성 
  const onInsert = useCallback(e => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
  }), [number, list]; // number, list가 바뀌었을 때만 함수 생성 

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```

### useCallback의 파라미터

- 첫번째 파라미터 : 생성하고 싶은 함수
- 두번째 파라미터 : 배열
    - 배열 : 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시
        - onChange - **비어있는 배열** ([]) : 컴포넌트가 **리렌더링될 때 만들었던 함수**를 게속해서 재사용
        - onInsert - **number, list** : **인풋 내용이 바뀌거나 새로운 항목이 추가될 때** 새로 만들어진 함수를 사용

함수 내부에서 **상태값에 의존**해야 할 때;

그 값을 반드시 **두번째 파라미터 안에 포함**시켜야 함

ex.

onChange; 기존의 값을 조회하지 않고 바로 설정만 함 → 배열이 비어있어도 돼

onInsert; 기존의 number와 list를 조회해서 nextList를 생성 → 배열 안에 number, list 꼭 필요



# 8.6 useRef

### useRef

함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 함

Average 컴포넌트에서 **등록** 버튼을 눌렀을 때 포커스가 input쪽으로 넘어가도록 코드 작성하기

(src > Average.js)

```jsx
import React, { useState, useMemo, useRef, useCallback } from 'react';

const getAverage = numbers => {
  console.log('평균값 계산 중..');
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');
  const inputEl = useRef(null);

  const onChange = useCallback(e => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링 될 때만 함수 생성
  const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
    inputEl.current.focus();
  }, [number, list]); // number 혹은 list가 바뀌었을 때만 함수 생성

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} ref={inputEl} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```

useRef를 사용해 ref를 설정하면, **useRef를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가르킴**

## 8.6.1 로컬 변수 사용하기

컴포넌트 로컬 변수를 사용할 때도 useRef 활용 가능

Q. 로컬변수?

A. 렌더링과 상관없이 바뀔 수 있는 값

### 클래스형 컴포넌트

로컬변수를 사용해야 할 때 다음과 같이 작성

```jsx
import React, { Component } from 'react';

class MyComponent extends Component {
    id = 1
    setId = (n) => {
        this.id=n;
    }
    printId = () => {
        console.log(this.id);
    }

    render() {
        return (
            <div>
                Mycomponent
            </div>
        );
    }
}

export default MyComponent;
```

### 함수형 컴포넌트

```jsx
import React, {useRef} from 'react';

const RefSample = () => {
    const id = useRef(1);
    const setId = (n) => {
        id.current = n;
    }
    const printId = () => {
        console.log(id.current);
    }

    return (
        <div>
            refSample
        </div>
    );
};

export default RefSample;
```

ref 안의 값이 바뀌어도 컴포넌트가 **렌더링 되지 않는다**는 점에서 주의!

**렌더링과 관련되지 않는 값을 관리**할 때만 이러한 방식으로 작성할 것~



# 8.7 커스텀 Hooks 만들기

여러 컴포넌트에서 비슷한 기능을 공유할 경우, 이를 **여러분만의 Hooks로 작성**해 **로직을 재사용** 가능

기존에 Info 컴포넌트에서 여러 개의 input을 관리하기 위해 useReducer로 작성했던 로직을 useInputsf라는 Hook으로 따로 분리해보기

(src > useInputs.js)

```jsx
import {useReducer} from 'react';

function reducer(state, action){
    return {
        ...state, 
        [action.name]: action.value
    };

    export default function useInputs(initialForm) {
        const[state, diapatch] = useReducer(reducer, initialForm);
        const onChange = e => {
            dispatch(e.target);
        };
        return [state, onChange];
    }
}
```

(src > Info.js)

```jsx
import React, {useReducer} from 'react';
import useInputs from './useInputs';

const Info = () => {
    const [state, onChange] = useInputs({
        name: '',
        nickname: ''
    });
    const { name, nickname } = state;
    
    return (
        <div>
            <div>
                <input name="name" value={name} onChange={onChange} />
                <input name="nickname" value={nickname} onChange={onChange} />
            </div>
            <div>
                <div>
                    <b>이름: </b> {name}
                </div>
                <div>
                    <b>닉네임: </b> {nickname}
                </div>
            </div>
        </div>
    );
};

export default Info;
```

⇒ 훨씬 깔끔한 코드!!



# 8.8 다른 Hooks

다른 개발자가 만든 Hooks도 라이브러리로 설치해 사용 가능

🔗 [https://nikgraf.github.io/react-hooks/](https://nikgraf.github.io/react-hooks/)

🔗 [https://github.com/rehooks/awesome-react-hooks](https://github.com/rehooks/awesome-react-hooks)



# 8.9 정리

Hooks 패턴 : 클래스형 컴포넌트 작성하지 않고도 대부분의 기능 구현 가능

그렇다고 해서 기존의 setState 방식이 잘못된 것은 아님!

리액트 매뉴얼에 따르면...

기존의 클래스형 컴포넌트는 앞으로도 계속해서 지원될 예정

다만, 매뉴얼에서 새로 작성하는 컴포넌트의 경우 **함수형 컴포넌트와 Hooks를 작성**할 것을 권장!!!

앞으로 프로젝트를 개발할 때는 함수형 컴포넌트의 사용을 첫번째 옵션으로 두고, 꼭 필요한 상황에서만 클래스형 컴포넌트를 사용할 것~!!!
