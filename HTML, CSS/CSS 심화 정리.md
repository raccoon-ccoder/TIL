### `Transitions`
- 어떤 상태에서 다른 상태로의 `변화`를 `애니메이션화` 하는 것
- `state`에 따라 바뀌는 `property`를 가지고 있어야 한다
- state에 따라 바뀌는 property를 다 애니메이션화 하고 싶을 경우 all, 아니라면 해당 property만 명시한다
- `state가 없는 쪽(root)`에 사용해야 한다 (element에 작성)

```css
a {
  color: white;
  background-color: tomato;
  transition: all 4s ease-in-out;
  /* 
  transition: color 3s ease-in, background-color 5s linear;
  */
}
a:hover {
  color: tomato;
  background-color: wheat;
 }
```
`ease-in function` 
- transition 맨마지막에 있는 옵션으로 브라우저에게 애니메이션이 `어떻게` 변할지 알려주는 역할이다
- 만약에 디폴트(linear, ease-in 등)가 아닌 나만의 애니메이션을 만들고 싶다면 `cubic-bezier` property를 사용한다
- [참고 사이트](https://matthewlein.com/tools/ceaser)
```css
transition: all 4s cubic-bezier(0.770, 0.000, 0.175, 1.000);
```

***
### `Transform`
- 원하는대로 `변형`하는 것
- 픽셀을 변형하는 것이기에 다른 box element, 이미지에 영향을 끼치지 않는다. 즉 `siblings에게 영향을 끼치지 않는다`

```html
<head>
       <style>
           img {
               border: 5px solid black;
               border-radius: 50%;
               width: 200px;
               height: 200px;
               transform: translateX(80px);
           }
        </style>
</head>
    
<body>
       <img src="cat.jpeg" />
       <span>Hello</span>
</body>
```
![스크린샷 2021-12-30 오후 11 14 51](https://user-images.githubusercontent.com/77538818/147759768-503277a2-dd4f-4af1-ba01-89ba68694c32.png)
- span, img 둘다 inline 요소인데 왜 img 요소만 움직여지고 span 위치는 고정된 것일까?
- transform은 픽셀을 변형하는 방법이라 span은 img가 원래 그 자리에 있다고 생각하기에 span의 위치가 옮겨지지 않았다. 따라서 사진처럼 span 위에 이미지가 겹쳐진 결과가 나왔다.

***
### `Animations`
- 계속되는 애니메이션을 만드는 방법
1. `@keyframes` 생성
2. 애니메이션을 사용할 element에 `animation` property 작성

```css
@keyframes superCoinFlip {
  0% { transform: rotateX(0); }
  50%{ transform: rotateX(360deg); }
  100% { transform: rotateX(0); }
}
img { animation: superCoinFlip 3s ease-in-out infinite; }
```
참고로 transform만으로 애니메이션을 만들 필요는 없지만 transform을 권장한다 (일부는 애니메이션으로 잘 안도기 때문)
[애니메이션 참고 링크](https://animista.net/)

***
### `Media query`
- 오직 CSS만을 이용해서 스크린의 사이즈를 알 수 있는 방법이다 (웹사이트를 보고 있는 사용자의 스크린 사이즈)
- 여러 조건을 추가하여 CSS를 적용할 수 있다 (media device, 높이, 너비 조건 등...)
```css
/* 스크린 장치이며 최소 너비가 800px이고 가로모드일경우 적용함 */
@media screen and (min-width: 800px) and (orientation: landscape){
  div { background-color: wheat; }
}

/* 브라우저 출력시 적용되는 경우로, 보통 프린트 컬러 잉크를 줄이기 위해 색상을 변경하게 된다 */
@media print { 
  body { background-color: white; }
}
```
  
           
           
           
