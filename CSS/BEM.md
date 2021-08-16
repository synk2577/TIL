# BEM Naming Convention

CSS class명 방법론 [🔗 ref](http://getbem.com/)

- Block - Element - Modifier
- class(.)만을 사용, id(#) 사용하지 않음

## Block
- 독립적인 컴포넌트     
ex) `header`, `container`, `menu`, `checkbox`, `input`


## Element
- 블럭의 요소
- `__` 사용: `block__element`   
ex) `menu item`, `list item`, `checkbox`, `caption`, `header title`


## Modifier
- block이나 element의 속성 
- `--` 사용: `block__element--modifier`, `block--modifier`    
ex) `disabled`, `highlighted`, `checked`, `fixed`, `size big`, `color yellow` 


### Using
```html
<button class="button">
	Normal button
</button>
<button class="button button--state-success">
	Success button
</button>
<button class="button button--state-danger">
	Danger button
</button>
```

```css
.button {
	display: inline-block;
	border-radius: 3px;
	padding: 7px 12px;
	border: 1px solid #D5D5D5;
	background-image: linear-gradient(#EEE, #DDD);
	font: 700 13px/18px Helvetica, arial;
}
.button--state-success {
	color: #FFF;
	background: #569E3D linear-gradient(#79D858, #569E3D) repeat-x;
	border-color: #4A993E;
}
.button--state-danger {
	color: #900;
}
```

## Benefits
- 재사용을 통해 css 코드 양을 줄일 수 있음 
- css 구조 이해가 쉬워짐  
