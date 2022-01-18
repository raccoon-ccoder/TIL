## JavaScript에서 CSS 편리하게 다루기 (기본)
JavaScript에서 CSS를 직접 변경하기 보다는 CSS 파일을 작성 후 JS에서 event 발생시 class명을 변경한다던지와 같은 간접적인 방빕을 사용하는 것이 더 좋다.
JavaScript 코드가 간결해지고 덜 헷갈리기 때문이다. 따라서 편리하게 다루는 기본적인 방법들을 정리해보았다.

### 1. CSS에서 클래스명마다 달리 작성 및 JavaScript에서 클래스명 변경하기 (`className` 이용)
```html
<div class="hello">
        <h1>Grab me!</h1>
</div>
```
```css
h1 {
    color: cornflowerblue;
    transition: color .5s ease-in-out;
}

.active {
    color: tomato;
}
```
```js
const h1 = document.querySelector(".hello h1");

function handleTitleClick() {
    const clickedClass = "active";
    if(h1.className === clickedClass){
        h1.className = "";
    }else{
        h1.className = clickedClass;
    }
}

h1.addEventListener("click", handleTitleClick);
```
![ezgif com-gif-maker](https://user-images.githubusercontent.com/77538818/149986166-fc08c8be-9102-4d3a-b6ca-41f6dc12a255.gif)
- CSS에서 active라는 클래스명에 color css를 변경한 후 JS에서 h1 태그 클릭시 `className` property를 이용해서 active라는 클래스명을 부여하고 제거하는 효과를 주었다.

### 오류 발생 케이스
```html
<div class="hello">
        <h1 class="cool-font">Grab me!</h1>
</div>
```
```css
h1 {
    color: cornflowerblue;
    transition: color .5s ease-in-out;
}

.active {
    color: tomato;
}

.cool-font{
    font-family: 'Courier New', Courier, monospace;
}
```
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/77538818/149987390-2b3ce11a-03c6-4055-9603-5ad3341da900.gif)
- h1 태그에 폰트 관련 클래스명을 추가한 경우 클릭 event 발생시 폰트 css가 사라지게 되는 오류가 발생한다.
- 이유는 위 JS 코드를 보면 h1 태그의 className이 active일때 클릭한 경우 `className`이 공백(`""`)으로 되기에 html에 작성한 cool-font 클래스명도 사라지게 된다.

### 2. JavaScript에서 클래스명 추가/삭제하기 (`classList` 이용)
```js
function handleTitleClick() {
    const clickedClass = "active";
    if(h1.classList.contains(clickedClass)){
        h1.classList.remove(clickedClass);
    }else{
        h1.classList.add(clickedClass);
    }
}
```
![ezgif com-gif-maker (2)](https://user-images.githubusercontent.com/77538818/149988163-071c1f33-ffe4-4716-ad53-b50e5358d855.gif)
- JS에서 className이 아닌 `classList` 라는 property를 사용하여 h1 태그의 모든 클래스명을 가지는 `배열`을 호출하여 active라는 클래스명이 존재할 경우 제거하고 없을 경우 클래스명을 추가하여 오류를 수정하였다.
- 하지만 아직도 코드가 간결해보이지 않는 찝찝함이 있다...

### 3. JavaScript에서 클래스명 변경하기 (`toggle()` 이용)
```js
function handleTitleClick() {
    const clickedClass = "active";
    h1.classList.toggle(clickedClass);
}
```
![ezgif com-gif-maker (3)](https://user-images.githubusercontent.com/77538818/149988821-349893db-1197-4eb6-b543-45f8aeedb1cb.gif)
- `toggle()` 이라는 메서드를 이용해서 classList 배열에 해당 클래스명이 있으면 제거하고 없으면 추가해주는 기능을 간결하게 구현하였다.
- `toggle()` : 하나의 매개변수만 있을 때 클래스 값을 토글링한다. 즉, 클래스가 존재하면 제거하고 false를 반환, 존재하지 않으면 클래스를 추가하고 true를 반환한다.
- 참고로 `toggle`의 뜻은 한번 눌렀을 때 어떤 기능이 ON되고, 다시 누르면 OFF되는 것을 의미한다.

