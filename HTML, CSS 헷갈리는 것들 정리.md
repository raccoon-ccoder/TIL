### :first-child, :last-child  선택자 사용 이유?
- 의사 클래스 선택자 (가상 클래스 느낌) 중 한 종류로 기존에 존재한 선택자로 구체적인 스타일을 줄 수 없기에 가상 클래스, 가상 요소로 구체적으로 스타일을 나타낼 수 있음

### `요소:first-child` { 속성: 속성값; } 
- 부모 요소 안에 있는 첫번째 자식만을 선택할 수 있는 선택자
	
```html
<h4>커피 메뉴</h4>
<div>
	<p>아메리카노</p>
	<p>카페라떼</p>
	<p>카푸치노</p>
</div>

<h5>옵션 선택</h5>
<ol>
	<li>샷 추가</li>
	<li>얼음 추가</li>
	<li>우유 변경</li>
</ol>
```

```css
p:first-child{ color : green; }
ol > li:last-child{ background-color : blue; }
```

<img width="242" alt="스크린샷 2021-12-27 오후 6 23 05" src="https://user-images.githubusercontent.com/77538818/147456984-2f67ce43-811a-4292-a216-710e341a1d93.png">

***

### flex container에서 flex children의 순서를 바꾸고 싶은 경우
1. `flex-direction: row-reverse`
``` html
<div  class="message-row message-row--own">
	<div  class="message-row__content">
		<div  class="message__bubble">
			<span  class="message__detail">Hi!</span>
			<span  class="message__time">21:22</span>
		</div>
	</div>
</div>
```
``` css
.message__bubble  {
display:  flex;
align-items:  flex-end;
flex-direction:  row-reverse;
}
```
2.  flex children에게 `order` 부여
``` css
.message-row--own  .message__detail  { order:  1; }
.message-row--own  .message__time  { order:  0; }
```
![스크린샷 2021-12-27 오후 10 39 32](https://user-images.githubusercontent.com/77538818/147478588-81f0ea37-4390-43a5-94f6-ef34d0e6080c.png)

사진과 같이 텍스트가 아닌 시간이 먼저 나오게 된다.

***

### 같은 html 코드를 사용하지만 css를 다르게 하고 싶은 경우
- css class 선택자를 변경하는 것이 아닌, `modifier`를 사용한다. 
``` html
<!-- 변경전 코드 -->
<!-- 상대방이 나에게 보내는 메세지 -->
<div  class="message-row">
	<img  src="img/cat.jpeg"  />
	
	<div  class="message-row__content">
		<span  class="message__author">냐옹이</span>
		<div  class="message__bubble">
			<span  class="message__detail">안녕하냐옹...</span>
			<span  class="message__time">21:22</span>
		</div>
	</div>
</div>

<!-- 내가 상대방에게 보내는 메세지 -->
<div  class="message-row">
	<div  class="message-row__content">
		<div  class="message__bubble">
			<span  class="message__detail">Hi!</span>
			<span  class="message__time">21:22</span>
		</div>
	</div>
</div>
```
``` css
.message-row  {
	width:  100%;
	display:  flex;
	margin-bottom:  20px;
}
```
![스크린샷 2021-12-27 오후 10 49 39](https://user-images.githubusercontent.com/77538818/147478592-1b5336d2-17e8-433b-a269-c4d156356185.png)

같은 component기에 같은 div 코드를 사용했지만 사진에는 말풍선의 위치가 같은 방향이라 내 말풍선은 위치를 반대로 변경해야 한다.


``` html
<!-- 변경 후 코드 -->
<!-- 상대방이 나에게 보내는 메세지 -->
<div  class="message-row">
	... 생략
</div>

<!-- 내가 상대방에게 보내는 메세지 -->
<div  class="message-row message-row--own">
	... 생략
</div>
```
``` css
.message-row--own  { justify-content:  flex-end; }
```
![스크린샷 2021-12-27 오후 11 03 23](https://user-images.githubusercontent.com/77538818/147478951-36c8ee02-7d2e-46dc-8c58-4d1e7566b974.png)

이럴때 해당 태그에 modifier `(message-row--own)` 를 사용해 클래스를 추가하여 별도의 css를 추가한다.

***

### CSS 애니메이션 효과 끝난 후 마지막 상태 유지
- `forwards` 키워드를 사용하면 애니메이션의 마지막 상태를 유지할 수 있다.

``` css
@keyframes hideSplashScreen {
    from {
        opacity: 1;
    }
    to {
        opacity: 0;
    }
}

#splash-screen {
    animation: hideSplashScreen 0.5s ease-in-out forwards;
    animation-delay: 1s;
}

```
위와 같이 css를 꾸밀 경우 투명도 1-> 0으로 되었다가 애니메이션이 끝나면 다시 투명도가 1로 되지만 _animation_ 속성에 `forwards` 속성값을 사용하면 투명도가 0으로 유지된다.

***

### CSS 애니메이션이 매끄럽지 못할 때 사용하는 방법
- `will-change` 키워드 사용
	- CSS 애니메이션이 약간 부자연스럽게 (매끄럽지 못하게) 동작하는 경우 해당 키워드 사용
	- 브라우저에게 어떤 애니메이션이 진행될 것이라고 미리 알려줌으로써 브라우저가 컴퓨터  그래픽카드를 이용해 애니메이션을 가속화

``` css
@keyframes heartBeat {
    0% {
        color: white;
        transform: none;
    }
    50% {
        color: tomato;
        transform: scale(1.3);
    }
    100% {
        color: white;
        transform: none;
    }
}

/* open-post__heart-count class 요소에 마우스를 올렸을 때 그 안에 있는 i 태그에게 css 적용 */
.open-post__heart-count:hover i {
    will-change: transform;
    animation: heartBeat 1s linear infinite;
}
```
위와 같은 경우에는 heartBeat 애니메이션이 살짝 떨리면서 작동하였지만 will-change 속성으로 자연스럽게 하트 이모지가 자연스럽게 심장 뛰는 효과를 나타낼 수 있다.

***

### Collapsing margins
- 두개의 box의 border가 같을 경우 (경계가 만나게 될 경우), box들의 margin은 하나로 취급하는 현상 (위나 아래의 border가 만나게 될 경우에만 발생)

``` html
<body>
      <div></div>
</body>
```
``` css
html { background-color: white; }

body {
	background-color: brown;
	margin: 10px;
}
           
div {
	height: 150px;
	width: 150px;
	background-color: gray;
	margin: 80px;
}
```
![스크린샷 2021-12-28 오후 7 41 31](https://user-images.githubusercontent.com/77538818/147558360-fc696725-a89b-4915-bc09-bed3923f6b5d.png)
body와 div의 border(경계)가 만나기에 두개의 margin은 하나로 합치게 되기에 body의 margin은 10px이지만 div의 margin은 80px이기에 body의 margin도 80px로 적용된다.
