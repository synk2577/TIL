# Javascript의 3가지 객체 유형
자바스크립트는 객체 기반 언어
1. [**코어** 객체](#코어-객체) : 언제 어디서나 사용 가능한 기본 객체   
  ex. `Array`, `Date` ...
2. [**HTML DOM** 객체](#HTML-DOM): HTML 문서에 작성된 각 HTML 태그들을 객체화 한 것   
  ex. `document`, `body` ...
3. [**브라우저** 객체](#브라우저-객체): 브라우저를 제어하기 위해 제공되는 객체   
  ex. `window`, `history` ...

<br>

## 코어 객체


<br>    

## HTML DOM 
Document Object Model   
목적: HTML 태그가 출력된 모양/콘텐츠 **제어**

### DOM 트리
- HTML 태그 포함 관계에 따라 DOM 객체의 tree 생성 -> 부모자식 관계 형성
- DOM 트리의 루트는 `document` 객체
- HTML 태그 당 DOM 객체가 하나씩 생성

### DOM 객체
- DOM 트리의 한 노드
- HTML 태그 당 하나의 DOM 객체 생성

### DOM 트리에서 객체 찾기
- **`document.getElementById()`** : id 속성으로 찾기
- **`document.getElementsByTagName()`** : 태그 이름으로 찾기
- **`document.getElementsByClassName()`** : class 속성으로 찾기

### `innerHTML` property
시작 태그와 종료 태그 사이에 들어있는 HTML 콘텐츠

### 브라우저가 HTML 태그를 화면에 그리는 과정
1. 브라우저가 document 객체 생성 -> DOM 트리의 틀
2. 브라우저가 HTML 태그를 읽고, DOM 트리에 DOM 객체 생성
3. 브라우저는 DOM 객체를 화면에 출력
4. HTML 문서 로딩이 완료되면 DOM 트리 완성
5. DOM 객체 변경 시, 브라우저는 해당 HTML 태그의 출력 모양을 바로 갱신

### HTML 페이지 로딩 과정
1. 브라우저는 HTML 페이지 로드 전 빈 상태 document 생성
2. 브라우저는 HTML 페이지를 위에서 아래로 해석
3. HTML 태그들을 document 객체에 담아감 (DOM 객체 생성)
4. `</html>` 태그를 만나면 document 객체를 완성하고 닫음


<br>

## document 객체
### `document`
- HTML 문서 전체를 대변하는 객체    
- DOM 객체를 접근하는 경로의 시작점    
- DOM 트리의 최상위 객체
  : document 객체를 root로 하여 DOM 트리가 생성   

### document 객체 접근
- `window.document`
- document 이름 접근
> document 객체는 DOM 객체가 아님‼️

### 메소드 활용
- **`document.write()`**: HTML 콘텐츠 마지막에 태그 추가
- **`document.writeln()`**: 줄바꿈 효과 추가
- **`document.open()`**: 새로운 HTML 페이지 시작 (document 객체에 담긴 DOM 트리를 지우고 새로 시작)
- **`document.close()`**: 브라우저에 출력된 HTML 페이지 완성

### 문서의 동적 구성
- **`document.createElement("태그이름")`**: DOM 객체 동적 생성
- **`부모.appendChild(DOM객체);`**: DOM 트리에 삽입
- **`부모.removeChild(삭제하려는자식객체);`**: DOM 객체 삭제


<br>

## 브라우저 객체

