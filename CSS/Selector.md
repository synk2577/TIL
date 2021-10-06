### line-height
텍스트를 싸고 있는 **라인의 기본 높이**를 설정		
이 값에 의해 **"라인 박스"의 높이**가 늘어나거나 줄어듦

[🔗ref](https://ohgyun.com/572)


<br>

### `rem` vs `em`
[🔗ref](https://medium.com/watcha/watcha-개발-지식-px-em-rem-f569c6e76e66)

- rem: 최상위 태그의 font-size 값이 기준
- em: 현재 요소의 font-size 값이 기준
	- em 을 사용해 스타일을 지정한 요소에 따로 font-size 값이 지정되지 않았다면, 해당 요소는 **부모요소로 부터 font-size 값을 상속(inherit)** 받을 것이며, em 은 그 상속 받은 값을 기준으로 삼게 됩니다

<br>


### white-space
**스페이스와 탭, 줄바꿈, 자동줄바꿈**을 어떻게 처리할지 정하는 속성 [🔗ref](https://www.codingfactory.net/10597)


<br>


### display: flex;
flex 아이템은 가로 방향으로 배치		
자신이 가진 내용물의 width만큼 차지 (inline 요소처럼)		
height는 컨테이너 높이만큼 늘어남		
⇒ height가 알아서 늘어나는 특징은 컬럼 레이아웃을 만들 때 아주 편리		

[🔗참고](https://studiomeal.com/archives/197)


<br>


### transition
[🔗 블로그 정리 참고](http://ielselog.blogspot.com/2013/09/understand-css-trasition.html)

아래 4가지 속성의 축약 속성으로 한 번에 4가지 표현 가능

```css
transition-property: width;
transition-duration: 1s;
transition-timing-function: ease;
transition-delay: .5s;

// 동일한 코드
transition: width 1s ease .5s;
```

- **`transition-property`**
    어떤 속성에 transition 효과 줄 지 결정

    - none
    - all: 모든 속성이 tranairion 효과를 얻음
    - property: 효과 얻게될 속성 지정
- **`transition-duration`**
    변화가 일어나는 시간 지정

- **`transition-timing-function`**
    변화가 일어나는 속도 지정

    - linear: 변화가 시작부터 종료까지 동일한 속도로 일어남
    - ease: 극초반은 느리게, 초반은 빠르게, 종료지점은 느리게
    - ease-in: 시작지점의 변화가 천천히 진행
    - ease-out: 종료지점의 변화가 천천히 진행
    - ease-in-out: 시작지점과 종료지점의 변화가 천천히 진행
    - `...`
- **`transition-delay`**
transition의 default 값 : `**all 0 ease 0**`


<br>


### box-sizing
요소의 **너비와 높이를 계산하는 방법** 지정

- **`content-box`**: width와 height 속성이 **콘텐츠 영역만 포함**하고 **안팎 여백과 테두리는 포함하지 않음**

- **`border-box`**: width와 height 속성이 **안쪽 여백과 테두리는 포함**하고, **바깥 여백은 포함하지 않음**

<table>
	<tr>
		<td></td>
		<td>content-box</td>
		<td>border-box</td>
	</tr>
	<tr>
		<td>콘텐츠 영역</td>
		<td>포함</td>
		<td>포함</td>
	</tr>
	<tr>
		<td>안쪽 여백</td>
		<td>x</td>
		<td>포함</td>
	</tr>
	<tr>
		<td>테두리</td>
		<td>x</td>
		<td>포함</td>
	</tr>
	<tr>
		<td>바깥 여백</td>
		<td>x</td>
		<td>x</td>
	</tr>
</table>                 

```css
.box {
	width: 350px; 
	border: 10px solid black;
}

// content-box 적용시, 요소의 너비는 350+10+10=370px
// border-box 적용시, 요소의 너비는 350px;
```

<br>


### 복합 Selector
- `space`: 후손 관계
- `>` : 자식 관계
- `+` : 인접 형제 관계 (바로 앞/뒤에 위치)
- `~` : 일반 형제 관계 (앞/뒤 관계만 성립하면 됨)


<br>


### block
- 항상 새 라인에서 시작
- 화면 너비 전체를 차지 (100%)
- width & height 크기 조절
- padding, margin 설정 가능

### inline
- 새 라인에서 시작 못함
- 옆에 다른 요소 배치 가능
- width & height 크기 조절 불가능 → content만큼 너비 차지
- padding, margin / -top, -bottom 설정 불가능
 
### inline-block 
- 새 라인에서 시작 못함
- 옆에 다른 요소 배치 가능
- width & height 크기 조절
- padding, margin 설정 가능


<br>


### gird
float 프로퍼티로 요소를 정렬할 수 있지만, flexbox, grid를 더 많이 사용

[🔗ref](https://ibrahimovic.tistory.com/23) 
- `grid-template-colums`
- `grid-template-rows`

```css
.wrapper {
    display: grid;
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 50px 50px;
}
```
> 3개의 column과 2개의 row로 표현      	     
> 각 칸은 너비 100px, 높이 50로 설정됨		


<br>


### clip
**이미지 요소의 일부분**만 보이도록 하기
```
clip: auto | shape | initial | inherit
```
- auto: 요소의 모든 부분이 나오도록 함
- shape: 특정 부분이 나오도록 함
- initial: 기본 값으로 설정
- inherit: 부모 요소의 속상값을 상속받음

특정 부분만 나오게 할 때 - 네모 모양의 영역		
% 사용 불가능, 단위는 px
```
clip: rect(<top>, <right>, <bottom>, <left>)
```
- top: 위를 기준으로 시작하는 위치
- right: 왼쪽을 기준으로 끝나는 위치
- bottom: 위를 기준으로 끝나는 위치
- left: 왼쪽을 기준으로 시작하는 위치

position 속성 값이 `absolute` or `fixed`일때만 적용됨		


<br>


### overflow
자식 요소가 부모 영역보다 더 클 때
- visible 
- scroll
- hidden
- auto

`overflow-x`와 `overflow-y` 속성으로 **가로/세로 축만 스크롤 생성**이 가능함!

#### overflow-x
- visible: 박스르 넘어가도 그대로 보여줌
- hidden: 부모 요소 범위르 넘어가면, 보이지 않음 (가로 스크롤바 나타나지 않음)
- scroll: 부모 요소 범위를 넘어가면, 스크롤바 나옴 (스크롤바 항상 표시)
- auto: 부모 요소 범위를 넘어가면, 보이지 않도로 하되 스크롤바 표시 (내용이 넘칠때만 스크롤바 표시)


<br>


### margin: auto
내가 가질 수 있는 여백을 모두 가짐
-> center 정렬시 상하좌우 여백을 auto로 설정시 각 여백을 모두 가지므로 center 정렬이 됨












