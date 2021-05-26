### Context API
**전역적으로 사용할 데이터**가 있을 때 유용   
리덕스, 리액트 라우터, styled-components와 같은 리액트 라이브러리에서도 많이 사용

ex) 사용자 로그인 정보, 애플리케이션 환경 설정, 테마 등 

<br> 

# 15.1 Context API를 사용한 전역 상태 관리 흐름 이해하기

리액트 프로젝트에서는 **`props`** 를 이용해 컴포넌트 간 데이터 전달    
여러 곳에서 필요한 데이터는 **최상위 컴포넌트인 App의 state**에 넣어서 관리!!    

- App 컴포넌트에서 하위 컴포넌트로 데이터 전달시 유지보수성 낮아지는 문제점 
- 리덕스, MobX, Context API 등으로 전역상태 관리 

※ 리덕스, MobX: 상태관리 라이브러리

<br> 

# 15.2 Context API 사용법 익히기

### `createContext`
- 새 Context를 만들 때 사용하는 키워드   
- 파라키터: 해당 Context의 기본 상태 지정

```jsx
import { createContext } from "react";

const ColorContext = createContext({ color: "black" });

export default ColorContext;
```

### `Consumer` 
Context 안에 들어있는 Consumer 컴포넌트를 사용 

#### Function as a child (Render Props) 
컴포넌트의 children 자리에 **함수를 전달**하는 것 (일반 JSX, 문자열 X)    

### `Provider`
- Context의 value 변경 가능
- value를 명시하지 않으면 오류 발생 (주의)

<br>

# 15.3 동적 Context 사용하기
Context 값을 업데이트하려면??    
Context의 `value`는 **상태 값** 외에도 **함수**가 올 수도 있음









































