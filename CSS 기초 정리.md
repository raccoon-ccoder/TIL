### `block` 특징
- 옆에 다른 block 요소가 올 수 `없다`
- 높이, 너비가 있다
- margin, border, padding을 가지고 있다
- div, article, form, footer, header, p 등..

### `inline` 특징
- 옆에 다른 inline 요소가 올 수 `있다`
- 높이, 너비가 없다 (box가 아니기 때문)
- border, padding을 가지며 `좌, 우에 대한 margin`만 존재한다 (높이, 너비가 없기 때문)
- inline 요소에 위,아래 margin을 주고 싶다면? -> 방법 중 하나로 inline요소들을 block로 변경
- a, button, img, span, textatea 등..

|  | block | inline-block | inline |
|:---:|:---:|:---:|:---:|
|줄바꿈 여부|O|X|X|
|기본적으로 갖는 너비(width)|100%|글자가 차지하는 만큼|글자가 차지하는 만큼|
|width, height 사용 가능여부|O|O|X|

*** 

### `margin`
- box의 border(경계)의 바깥에 있는 공간

### `padding`
- box의 경계로부터 안쪽에 있는 공간 

### `border`
- box의 경계선

***

### `display` 속성
- block 속성을 inline, 혹은 inline 속성을 block으로 변경할 수 있는 속성
- 속성값으로 `block`, `inline`, `inline-block`, `flex` 등이 있다

### `display:inline;`
- box 요소를 inline으로 바꿔주며 `width, height를 가질 수 없다`
- 만약 해당 태그에 width, height가 있다면 화면에 보여지지 않는다 (만약 태그 안에 content가 있다면 content 기준으로 높이, 너비 결정됨)

### `display:inline-block;`
- inline의 단점을 보완하는 느낌의 속성으로 width, height를 설정할 수 있다
- 반응형 디자인을 지원하지 않고 요소 사이에 여백이 생기는 단점이 존재한다

### `display:flex;`
- 박스들을 어떤 곳이든 둘 수 있다는 장점이 있다
- 2차원 레이아웃에서 잘 작동한다

### `display:flex;` 사용시 규칙
1. 자식 엘리먼트(flex하게 만들 요소)가 아닌 `부모 엘리먼트` 에 css를 명시한다. 그리고 빈공간 등 옵션은 자식 엘리먼트가 아닌 부모 엘리먼트에 명시한다.
2. flex cotainer는 `주축(main axis)` 을 가지며 주축에 대한 수평에 관련된 css 설정을 할 수 있다 (ex : justify-content)
3. flex cotainer는 `교차축(cross axis)` 을 가지며 수직에 관련된 css 설정을 할 수 있다 (ex : align-items)

```html
 <body>
      <div></div>
      <div></div>
      <div></div>
</body>
```
```css
body {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```
3개의 div를 flex하게 하고 싶을 경우 div가 아닌 div의 부모인 body를 flex container로 명시한다.

### `display:flex;`일 경우 다양한 `property`
- `flex-direction:column;`일 경우 주축, 교차축이 바뀐다 (주축 -> 교차축, 교차축 -> 주축으로 되며 default는 flex-direction: row; )
- 부모가 flex container 이지만 자식도 flex container 일 수도 있다.
```html
<head>
   <style>
            body {
                display: flex;
                width: 300px;
                height: 300px;
                justify-content: space-between;
                align-items: flex-end;
            }
            div {
                display: flex;
                justify-content: center;
                align-items: center;
                width: 50px;
                height: 50px;
            }
        </style>
</head>

<body>
      <div>1</div>
      <div>2</div>
      <div>3</div>
    </body>
```
div는 body라는 flex container의 자식 요소이지만 동시에 div 안에 또 다른 요소가 있기에 그 요소들을 flex 하기 위해 div를 flex container로 명시할 수 있다.

***

### `position` property
- 레이아웃보다는 위치를 조금씩 옮기고 싶을 경우 사용하는 property

1. `position:fixed;`
- 부모 컨테이너에 속박되지 않고 `브라우저를 기준` 으로 어떤 위치에 고정시킬 수 있다
- margin, block 상관 없이 레이어를 부수고 다른 레이어 위에 위치할 수 있다는 의미

2. `position:relative;`  
- element가 `처음 위치한 곳` 을 기준으로 top, bottom, left, right를 이용해 위치를 수정한다

3. `position:absolute;`
- 가장 가까운 `relative 부모` 를 기준으로 이동한다
- 가장 가까운 순서로 relative한 부모 엘리먼트를 찾는데 없을 경우 최종적으로는 **body**가 부모 엘리먼트가 된다

※ `float`, `position: absoulte, fixed` 속성이 있는 요소는 display:flex, inline-flex를 제외하고 `display: block`으로 변경된다   
→ 따라서 span 태그에 position: fixed 속성이 있을 경우 block 요소로 바뀌며 width, height, 위아래 margin 값을 가질 수 있다   

***

### `pseudo selector`
- 단지 id, class 선택자가 아닌 세부적으로 엘리먼트를 선택할 수 있게 해준다

1. `:nth-child(n)`
- 형제 태그 목록에서 n번째 태그를 수정하는 것으로 n자리에는 `숫자`, `even`(홀수), `odd`(짝수), `2n+1` 처럼 설정할 수 있다 (ex : `span:nth-child(2n+1)` )

2. `CSS attribute selector`
- 주어진 특성의 존재 여부나 그 값에 따라 요소를 선택

`[attr]`
- `attr` 이라는 이름의 특성을 가진 요소 선택

`[attr=value]`
- attr이라는 이름의 특성값이 `정확히` value인 요소 선택

`[attr~=value]`
- attr이라는 이름의 특성값 중 value인 요소를 선택 
- ex ) [name~=hello] : `앞뒤에 공백이 있는 상태에서` hello인 경우만 선택 (name="good hello bye")

`[attr*=value]`
- attr이라는 특성값을 가지고 있으며, 값 안에 value리는 `문자열`이 적어도 `1개 이상` 존재한다면 요소를 선택
- ex ) [name*=hello] : 문자열 안에 hello가 있는 요소인 경우 선택 (name="GooDhelloBye")

`[attr^=value]`
- attr이라는 특성값을 가지고 있으며, `접두사`로 value가 값에 포함되어 있으면 요소를 선택

`[attr$=value]`
- attr이라는 특성값을 가지고 있으며, `접미사`로 value가 값에 포함되어 있으면 요소를 선택

```css
a[href*="example"] {
  background-color: silver;
}
```

3. `::placeholder` - placeholder의 특성만 바꾸고 싶은 경우 ( ex : input::placeholder )
4. `::selection` - 클릭해서 드래그 하는 상태 ( ex : p::selection )
5. `::first-letter` - 첫 글자에만 적용 ( ex : p::first-letter )

*** 

### `CSS 연결자` (Conbinators)
- 연결자는  선택자들 사이의 관계를 설명해주는 것이다 (선택 요소의 형제 중 이전 요소는 선택 불가하며 js를 사용해야 한다)

1. 자손 선택자 (`space`) 
- div span => div 밑에 있는 모든 span을 선택 (직접적인 부모가 아니어도 밑에 있는 모든 span이 선택)
2. 자식 선택자 (`>`) 
- div > span -> div 바로 밑에 있는 span을 선택
3. 인접 형제 선택자 (`+`)
- p + span -> p 요소 바로 뒤에 놓인 span 선택 (p 바로 뒤에 span이 아닌 다른 태그가 있다면 적용 X)
4. 일반 형제 선택자 (`~`)
- p ~ span -> p 요소의 다음 형제들 중 모든 span 선택 (p 바로 뒤에 오지 않아도 선택된며 이전 요소는 선택 불가)

***

### `States`
- 웹페이지의 조건에 맞춰 활성화를 가능하게 한다

1. `:active` - 해당 요소를 `클릭`하고 있는 상태
2. `:hover` - `마우스`가 대상 위에 있을 때의 상태
3. `:focus` - `키보드`로 선택되었을 때의 상태 (active와 비슷함)
4. `:visited` - `링크`를 1번이라도 방문했을 경우 스타일 적용
5. `:focus-within` - focus된 자식을 가진 `부모` element 상태

```css
/* form 태그(부모) 내부에 focus된 요소(자식)가 있다면 form(부모) 태그 css 적용 */
form:focus-within {
  border: 3px solid brown;
}

/* form 태그(부모) 내부에 focus된 요소(자식)가 있다면 form 내부의 input 태그 css 적용 */
form:focus-within input{
  border: 3px solid brown;
}

/* form에 마우스가 올라가있는 상태 & input이 focus되었을 때 2가지 상황 동시에 만족하는 경우 */
form:hover input:focus {
  background-color:  seagreen;
}
```

***

### `Variables`
- Custom Property라고도 하며 `변수`를 사용해 중복된 css 코드를 줄여준다

1. `:root` 라는 document의 뿌리인 element에 변수 추가 (`--변수명`)
2. 필요한 곳에 변수를 사용 - `var(--변수명)`

```css
:root {
  --main-color: #ffcc00;
  }
  
  p {
    color: var(--main-color);
  }

