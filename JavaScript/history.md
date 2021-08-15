# 자바스크립트의 역사

- 1993: UI 요소가 더해진 **Mosaic Web Browser**의 등장

- 1994: **Marc Andreessen**가 Netscape 회사 설립 후, **Netscape Navigator**의 탄생   
  HTML과 CSS로 정적이고 간단한 웹 페이지만 제작 가능    
  80% 점유율로 급격한 성장 -> 하지만, `Dynamic한 웹사이트`를 만들기 위한 고민   
  - [ ] 대안 1) JAVA - 웹 개발자가 사용하기에 무겁고 어려움  
  - [x] 대안 2) Scheme 언어를 변형해 사용    
      
- 1994: **Mocha**의 탄생 -  **Brendan Eich**가 프로토타입 베이스의 유연한 언어를 개발       
  추후 **LiveScript**로 이름 변경   
  Netscape Navigator 안에 LiveScript 엔진이 포함된 브라우저 출시    
  -> 웹 개발자들이 LiveScript 언어를 이용해 웹 페이지를 만들고 Netscape Navigator Browser가 언어를 실행하고자 하는 돔 요소에 대한 조작이 가능해짐    

- 1995: **JavaScript**로 이름 변경   
  JavaScript와 언어를 이해할 수 있는 엔진이 포함된 Netscape Navigator Browser의 출시   
  
- 1995: **Microsoft**사의 **Internet Explorer Browser** 탄생    
  JavaScript와 엔진의 소스코드를 Reverse Engineering해 JScript로 이름을 변경해 시장에 출시  

- 1996, 11: ECMA International 단체에 표준안을 제안

- 1997, 7: **ECMAScript1** 출시  
  **브라우저에서 동작하는 언어를 만들 때 문법적인 사항을 정리한 문서**
 
- 1998: ECMAScript2 

- 1999: ECMAScript3  
  error handling, 관계 연산자

- 2000: ECMAScript4  
  optional type annotation, class, enterprise scale
  
- 2000: Microsoft사의 Internet Explorer 시장 점유율 95% 차지  
  ECMAScript 표준안에 불참하기 시작
  
- 2004: **Nozilla**사의 **Firefox** Browser 출시  
  ECMA 단체에 AcrionScript3와 Tamarin 엔진에 대한 표준안 제안 -> 3사의 치열한 싸움
  
- 2004: Jesse James Garrett의 **AJAX 도입**(Asynchronous JavaScript and XML, 비동기적으로 데이터를 서버에서 받아오고 처리할 수 있도록 도움)

- ECMAScript 표준화를 앞두고, 다른 브라우저들도 등장  
  웹 시장의 성장함에 따라 개발자 커뮤니티가 등장  
  **jQuery, dojo, mootools 등의 라이브러리 등장 -> 라이브러리들의 공통점: 다른 브라우저의 구현사항을 신경쓰지 않는 것!**  
  
- 2008: **Google**사의 **Chrome Browser** 출시
  JIT(just-in-time compilation) 엔진이 포함된 강력한 브라우저로 **JavaScript를 실행하는 속도가 엄청 빠른 강력한 엔진**을 포함  
  
- **2009: ECMAScript5**  

- **2015: ECMAScript6**   
  default parameter, class, arrow function, const, let 등의 문법 정의   
  이후 매년 표준안이 개선됨
  
- 2016: ECMAScript7 

- 2017: ECMAScript8  

- 2018: ECMAScript9  

- 2019: ECMAScript10  


### JavaScript
Mature, Settled down  
모든 브라우저들이 ECMAScript의 표준 사항을 따름 
-> 이제는 라이브러리의 도움 없이, JavaScript와 Web APIs에서 제공하는 api만으로도 모든 브라우저에서 잘 동작하는 웹 사이트/애플리케이션 개발이 가능  

브라우저마다 ECMAScript의 표준안을 따라가는 엔진들이 존재
- Chrome - V8
- Firefox - SpiderMonkey
- Safari - JSCore
- MS Edge - Chakra
- Opera - Carakan
- Adobe Flash - Tamarin

### V8 엔진
node.js와 ELECTRON에서도 사용되어 짐  
Microsoft사에서 V8엔진을 대체 사용 (2020~)  

### BABEL
다양한 사용자들이 다양한 브라우저를 사용하지만, **개발자들은 최신 버전의 ECMAScript로 개발**하는 것이 효율적  
개발시에는 최신 버전의 ECMAScript를 쓰고, 사용자에게 배포시 JavaCript transcompiler를 이용해 ES5나 ES6로 변환된 코드를 생산해주는 것


<br>


# 자바스크립트의 동향
### SPA  
Single Page Application으로 하나의 페이지 안에서 데이터를 받아와 **필요한 부분만 부분적으로 업데이트**  
JavaScript 만으로 개발 가능하지만, React, AGULAR, Vue, Backbone 라이브러리/프레임워크를 이용해 SPA를 보다 쉽게 구현  

### Node.js
JavaScript는 브라우저에서 동적인 요소를 추가하기 위해서 만들어진 언어이고, ECMAScript의 활발한 표준화와 강력한 V8 자바스크립트 엔진을 통해 Node.js의 등장  
**V8 자바스크립트 엔진을 이용한 백엔드 서비스를 구현**할 수 있도록 만들어짐 + **Mobile/Desktop Application 개발**도 가능해짐   


JavaScript는 **브라우저에서 동작할 수 있는 유일한 언어였지만**!!
**WA(WebAssembly)의 등장**으로 Rust, C, C++, C#, JAVA, Python, Go 등 **다양한 언어를 이용해 웹 애플리케이션**을 만드는 것이 가능해짐  


<br>

#  Vanilla JS
어떠한 라이브러리나 프레임워크를 사용하지 않는 자바스크립트    

### 특징
- 다른 프레임워크보다 빠름
- 크로스 브라우징이 잘됨 (웹표준을 지키는 웹브라우저에 한함)
- 용량이 매우 가벼움
