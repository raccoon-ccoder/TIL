## [transition] display none일때 transition이 안되는 이유
자바스크립트를 이용해 시각적으로 요소가 보였다 안보였다 하는 작업을 할 경우, display를 none, block으로 제어할 때 transition이 제대로 작동하지 않는 경우가 있다.

```html
<button>click</button>
<div></div>
```
```css
div {
  display: none;
  width: 100%;
  height: 0vh;
  margin-top: 10px;
  background: gray;
  transition: all 0.3s;
}
.move {
  display: block;
  height: 50vh;
}
button {
  background-color: black;
  padding: 10px;
  border: transparent;
  border-radius: 10px;
  color: #fff;
}
```
```js
const btn = document.querySelector('button');
const box = document.querySelector('div');

// div 클릭 시 act 클래스 토글
btn.addEventListener('click', () => {
  box.classList.toggle('move');
})
```

![ezgif com-gif-maker](https://user-images.githubusercontent.com/77538818/156949856-d291171c-6e87-45d8-b5de-8e32cc8d68c6.gif)

 `transition`이 있음에도 불구하고 부드러운 효과가 아닌 딱 끊어지는 효과를 볼 수 있다. 왜 그런걸까?
 
 <br/>
 
 ### display none, block일 때 일어나는 일들
 요소가 화면에 출력되기 위해서는 브라우더 렌더링 엔진은 HTML,CSS를 파싱 후 각각 DOM, CSSOM을 생성 후 이들을 결합하여 `렌더 트리`를 생성한다.  
 이 렌더 트리는 렌더링을 위한 트리 구조의 자료구조로 HTML 요소들의 레이아웃을 계산하는 데 사용되며 브라우저 화면에 픽셀을 렌더링하는 페인팅처리에 입력된다. 
 
 <br/>
 
 이때, 렌더 트리는 화면에 출력되지 않아도 되는 일부 노드(`meta`, `script` 등)와 CSS에 의해 표시되지 않는(`display:none`) 노드들은 렌더 트리에서 생략된다.
 
 <br/>
 
 `transition`은 두 상태에서 속성의 변화가 일정 기간에 걸쳐 일어나도록 하지만 
 `display: none` 상태에서 동적으로 클래스를 추가하여 `display: block`을 추가한다고 하면 렌더 트리에 없다가 추가되는 것이기에 `transition`의 처음 시작점이 없다.    
 그래서 시각적으로 `transition`이 동작되지 않는 것처럼 보이게 된다.
 
 <br/>
 
 ### transition이 동작하게끔 하는 방법
 2가지의 선택사항이 있는데,
  - 컨텐츠만 시각적으로 숨기지만 접근성 트리에는 있어야 하는 경우
  - 컨텐츠도 시각적으로 숨기고 접근성 트리에도 없어야 하는 경우       
 이렇게 2가지로 예를 들어보자
 
 #### (1) 컨텐츠만 시각적으로 숨기지만 접근성 트리에는 있어야 하는 경우
 이 경우에는 시각적으로 화면에 요소는 보이지 않지만 접근성 트리에 있어야 하고 이럴땐 `display: none, block`으로 처리하면 렌더 틀에도 없기 때문에 접근성 트리에도 구현되지 않는다.   
 즉, 렌더 트리에도 있어야 하지만 요소를 시각적으로 숨겨야 한다.
 
#### 1. height
```css
div {
  width: 100%;
  height: 0vh;
  margin-top: 10px;
  background: gray;
  transition: all 0.3s;
}
.move {
  height: 50vh;
}
```
#### 2. transform
```css
div {
  width: 100%;
  height: 50vh;
  transform: scaleY(0);
  margin-top: 10px;
  background: gray;
  transition: all 0.3s;
}
.move {
  transform: scaleY(1);
}
```
#### 3. opacity
```css
div {
  width: 100%;
  height: 50vh;
  opacity: 0;
  margin-top: 10px;
  background: gray;
  transition: all 0.3s;
}
.move {
  opacity: 1;
}
```

#### (2) 컨텐츠도 시각적으로 숨기고 접근성 트리에도 없어야 하는 경우
대부분의 경우, 요소를 시각적으로 숨긴다는 것은 컨텐츠를 숨긴다는 것을 의미하는데 물론 스크린 리더 또한 읽히면 안된다는 것을 의미한다고 생각한다. 이럴땐 visibility 속성을 이용한다.
```css
div {
  visibility: hidden;
  width: 100%;
  height: 0vh;
  margin-top: 10px;
  background: gray;
  transition: all 0.3s;
}
.move {
  visibility: visible;
  height: 50vh;
}
```
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/77538818/156949877-bc6d48a8-993f-40f2-b38e-4fb02036b8cd.gif)
