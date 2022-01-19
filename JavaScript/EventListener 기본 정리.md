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

### event obejct 알아보기
- 모든 `EventListener function`의 첫번째 argument는 항상 지금 막 벌어진 `event에 대한 정보`가 제공된다.
- 브라우저는 `event`에 대해서 내가 필요로 할만한 정보(event obejct)를 제공한다.
- 위에 작성했던 function을 보면 매개변수가 없지만 매개변수를 추가하면 event에 대한 정보를 탐색할 수 있다. (event 정보가 필요없다면 argument 사용할 필요 X)
```html
<form class="login-form">
        <input 
            type="text" 
            required
            maxlength="15"
            placeholder="What is your name?" 
        />
        <input type="submit" value="Log In">
 </form>
```
```js
const loginForm = document.querySelector(".login-form");
const loginInput = document.querySelector(".login-form input");

function onLoginSubmit(event) {
    event.preventDefault();
    // preventDefault() : 어떤 event의 기본 행동이든지 발생되지 않도록 막아줌 
    // form submit의 경우 브라우저는 기본적으로 페이지를 새로고침함 ->  새로고침 막아줌
    // a 태그 link의 경우 click 하면 다른 페이지로 이동 -> 페이지 이동을 막아줌
    console.log(event);
}

loginForm.addEventListener("submit", onLoginSubmit);
```
![스크린샷 2022-01-20 오전 1 10 09](https://user-images.githubusercontent.com/77538818/150169923-a48bc2e7-2f5e-4ab0-b82b-a79a2acfdd1b.png)
- 이름을 입력 후 submit 버튼을 누르면 콘솔창에 위와 같이 뜨는데 `SubmitEvent`에 대한 정보들이 제공되는 것을 볼 수 있다.   

**Q) `preventDefault()`를 사용한 이유?**
- 해당 메소드는 어떤 event의 기본 행동이든지 발생되지 않도록 `막아주는 역할`을 한다.
- form submit 경우, 클릭시 브라우저는 기본적으로 페이지를 새로고침하게 되는데 위에선 `event object`를 알아보기 위해 페이지 새로고침을 막으려고 사용했다.
- event가 실행되기 전 어떤 특정 조작을 하고 싶을 때 (제어를 하거나 멈추고 싶을 때) `preventDefault()` 메소드를 유용하게 쓸 수 있다.


### <참고>
### `document`
- 브라우저에 이미 존재하는 `object`로 `HTML` 정보를 담고 있다. JS와 HTML은 이미 연결되어 있기에 브라우저가 제공하는 document라는 object 객체를 사용하여 HTML element를 가져와 사용할 수 있다.
- 즉 `HTML`를 `JavaScript` 관점에서 보는 셈이다.
