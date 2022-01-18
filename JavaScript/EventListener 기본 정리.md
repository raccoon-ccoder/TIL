## EventListener 기본 정리
브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킨다. 이때 이벤트가 발생했을 때 `호출될 함수`를 `event listener`라고 한다.

### event 작성 및 사용 방법
1. `event listener` 함수 작성
2. `addEventListener` 호출
3. `addEventListener()` 메서드의 매개변수로 listen하고 싶은 event, event 발생시 호출할 function(event listener) 작성
- 이때 함수의 매개변수를 통해 다른 함수의 내부로 전달받는 함수를 `콜백함수`라 하며 여기서 콜백함수는 handleTitleClick(), handleMouseEnter()이 된다
```js
const h1 = document.querySelector(".hello:nth-child(2) h1");

// 1. event listener 함수 작성
function handleTitleClick() {
    h1.style.color = "blue";
}

function handleMouseEnter() {
    h1.innerText = "Mouse is here!"
}

// 2. addEventListener 호출
h1.addEventListener("click", handleTitleClick);
h1.addEventListener("mouseenter", handleMouseEnter);

// event 호출방법 2
h1.onclick = handleTitleClick;

// window : document가 js에서 기본적으로 제공되듯이 window도 기본적으로 제공
function handleWindowResize() {
    document.body.style.backgroundColor = "tomato";
}

window.addEventListener("resize", handleWindowResize);
```
Q) 원래 함수 실행하려면 `()` 괄호가 필요한데 콜백함수에 작성하지 않은 이유는 ?
- 내가 직접 실행버튼을 누르는 것이 아니라 JS에게 function을 넘겨줘 나 대신 JS가 실행버튼을 누르게 `위임`하는 것이다.

### <참고>
### `document`
- 브라우저에 이미 존재하는 `object`로 `HTML` 정보를 담고 있다. JS와 HTML은 이미 연결되어 있기에 브라우저가 제공하는 document라는 object 객체를 사용하여 HTML element를 가져와 사용할 수 있다.
- 즉 `HTML`를 `JavaScript` 관점에서 보는 셈이다.
