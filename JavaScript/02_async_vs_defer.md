# `async` vs. `defer`
> HTML에서 JavaScript를 어떻게 효율적으로 포함할 수 있을까?  
> **사용자에게 어떤 순서로 페이지가 보여지는가?**  
<br>

### Case1) `<head>` 내부에 `<script>`가 포함되어 있는 경우
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="main.js"></script>
  </head>
  <body></body>
</html>

```
html을 한 줄씩 parsing 하다가 script 태그가 보이면 main.js 파일을 서버에서 다운로드 받은 후 실행한 다음에 parsing 작업을 재시작함  
> **parsing HTML** -> blocked (**fetching js** -> **executing js**) -> **parsing HTML** -> _"page is ready!"_  
- 단점) js 파일의 사이즈가 매우 크거나 인터넷 속도가 느린 경우 사용자가 웹사이트를 사용하는데 오랜 시간 소요   
<br>
<br>

### Case2) `<body>` 내부의 가장 마지막에 `<script>`가 포함되어 있는 경우
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div></div>
    <script src="main.js"></script>
  </body>
</html>

```
브라우저가 html을 다운받아 parsing 하고 페이지가 준비가 된 다음, script를 서버에서 받아와 실행  
페이지가 사용자들에게 js를 받기 전에 준비가 되므로 페이지의 컨텐츠를 볼 수 있음  
> **parsingHTML** -> _"page is ready!"_ -> **fetching js** -> **excuting js**   
- 장점) 사용자가 기본적인 html contents를 볼 수 있음    
- 단점) 웹사이트가 js에 의존적이라면, js를 이용해 서버에 있는 데이터를 받아오거나 돔 요소를 꾸미는 웹사이트의 경우 사용자가 정상적인 웹사이트를 보기 전까지는
fetch하고 execute하는 시간을 기다려야 함
<br>
<br>

### Case3) `<head>` 내부에 `<script>`가 포함되어 있고 `asyn` 속성값 사용  
asyn 속성값 (boolean 타입, default: true)  
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script async src="main.js"></script>
  </head>
  <body></body>
</html>

```
> **parsing HTML** -> blocked         -> **parsing HTML** -> _"page is ready!"_ 
> **fetching js** -> **executing js**

브라우저가 html를 다운받아 parsing하다가 병렬로 js파일을 다운로드 받고 완료되면 parsing을 멈추고 다운로드된 js파일을 실행하게 됨  
실행을 다하고 나서 나머지 html 파일을 parsing하게 됨
- 장점) 다운로드 시간 단축
- 단점) html이 parsing되기 전에 실행되기 때문에 js 파일에서 원하는 요소가 아직 정의되지 않을 수도 있고, js를 실행하기 위해 block 상태가 될 수 있기 때문에 사용자가 페이지를 보는데 시간이 걸릴 수 있음   
<br>
<br>

### Case4) `<head>` 내부에 `<script>`가 포함되어 있고 `defer` 속성값 사용  
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script defer src="main.js"></script>
  </head>
  <body></body>
</html>

```
> **parsing HTM**L -> _"page is ready!"_ -> **executing js**
> **fetching js**  

html을 parsing하다가 js를 파일을 다운로드만 하고 페이지 parsing이 완료된 후 사용자에게 페이지를 보여준 다음에 js 파일 실행    
<br>
<br>

### 다수의 js 파일을 script에 선언하는 경우 `aysnc`와 `defer`의 차이점
async | defer 
----- | ------
정의된 순서와 상관 없이 먼저 다운로드되는 파일을 실행 | js 파일을 다운로드 받은 후 순서대로 실행
js 파일이 순서에 의존적인 경우 문제 발생 | 원하는 순서대로 스크립트가 실행

###### `defer` 를 이용한 script의 선언이 제일 효율적임!
<br>
<br>

### js파일의 `'use strict';`의 사용
js는 유연한 언어지만, 위험하기도한 언어   
선언되지 않은 변수에 값을 할당하거나 기존에 존재하는 프로토타입을 변경하는 비상식적인 행위가 가능함!   

**ES5**에 추가된 내용    
**js 파일 상단에 `'use strict';` 선언**   
strict 모드로 개발할 것을 권장 -> **상식적인 범위 내에서 js를 이용하게 되며, js 엔진이 효율적으로 분석 가능해짐!**   







