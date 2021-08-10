# Bootstrap

### 정렬 
**정렬 가능한 태그**는 `inline 속성`이 부여된 요소여야 함
- `inline`
- `inline-block`

가운데 정렬
- 가로 정렬: `.justify-content-center`
- 세로 정렬: `.align-items-center`

대부분의 정렬은 **부모에게 정렬 기준을 부여**해 그 자식들을 정렬함     
→ 그 정렬기준은 자식 요소들에게 모두 적용!!


<br> 

### `.d-flex`
- `display: flex;`    
- 자식 태그들이 모두 `inline`으로 바뀌며 원하는데로 정렬 가능함 (후손 x, 세로 정렬 포함)


<br> 

### `.container`
- Bootstrap은 최상단에 Container가 있다고 가정함!   
- Container로 요소들을 감싼다  


<br> 

### `.row`/`.col` (Grid)
- 요소요소 마다 가로로 12등분
- 부모 요소에 `.row` 적용 후, 자식 요소에 `.col-{n}` 적용
  - 단, 한 요소에 `row`와 `col` 동시 사용 불가능   
- `.container` > `.row` > `.col-{n}` 순으로 적용해야 함


<br> 

### `<br>`
마우스로 드래그시 block으로 선택되기 때문에 br 태그의 사용을 권장하지 않음
