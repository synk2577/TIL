### line-height
텍스트를 싸고 있는 **라인의 기본 높이**를 설정		
이 값에 의해 **"라인 박스"의 높이**가 늘어나거나 줄어듦

[🔗ref](https://ohgyun.com/572)



### rem, em

**rem**
- root em

**em**



### white-space
**스페이스와 탭, 줄바꿈, 자동줄바꿈**을 어떻게 처리할지 정하는 속성 [🔗ref](https://www.codingfactory.net/10597)


### display: flex;
flex 아이템은 가로 방향으로 배치		
자신이 가진 내용물의 width만큼 차지 (inline 요소처럼)		
height는 컨테이너 높이만큼 늘어남		
⇒ height가 알아서 늘어나는 특징은 컬럼 레이아웃을 만들 때 아주 편리		

[🔗참고](https://studiomeal.com/archives/197)



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


### inherit 값
부모 속성으로부터 해당 요소의 값을 받아옴



### 복합 Selector
- `space`: 후손 관계
- `>` : 자식 관계
- `+` : 인접 형제 관계 (바로 앞/뒤에 위치)
- `~` : 일반 형제 관계 (앞/뒤 관계만 성립하면 됨)


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

