## box-sizing_padding 설정시 width 변하는 문제

만약 `width: 200px`, `height:100px`, `padding:20px`, `border:5px solid black`인 요소가 있을 경우 화면 상에는 어떻게 나타날까 ?    

![스크린샷 2022-01-09 오후 8 50 31](https://user-images.githubusercontent.com/77538818/148680990-63dc7e63-6cab-49db-9a7a-a8430da27e78.png)

## content-box
결론은 오른쪽 사진처럼 그려지게 된다. width를 200px로 설정했을 경우, 여기에 `padding과 border-width가 더해지기 때문`이다.
이런 계산 방법을 `content-box`라고 하며, `box-sizing`속성을 `content-box`로 주게 되면 이와 같이 계산하게 되며 default값이다.   

## border-box
왼쪽처럼 전체 박스에 변형을 가하지 않으려면 아래와 같이 CSS에서 `box-sizing`을 `border-box`로 지정하면 된다.
가로, 세로 너비가 마진 영역을 제외하고 `콘텐츠 영역`, `패딩 영역`, `보더 영역`을 `포함한 크기`로 설정한다.
```css
  box-sizing: border-box;
```

## content-box에서 width: 100%일 때의 문제
CSS에서 width 속성을 100%로 주면 부모의 width 만큼 너비가 설정된다.
하지만 content-box일 때 `width:100%`에 이어 padding이나 border를 주게 되면 부모 영역을 초과해서 너비가 정해지는 문제가 생길 수 있다.

이런 문제를 해결하기 위해 `box-sizing`을 `border-box`로 설정하거나 width를 `auto`로 설정하여 해결 할 수 있다.
width의 `기본 값`은 `auto`이므로 width를 아예 적어주지 않아도 정상적으로 동작한다.

```html
  <style>
.container1 {
    width: 300px;
    border: 5px solid black;
    background-color: white;
}
.container1 > div {
    padding: 20px;
    background-color: #ddd;
    margin-bottom: 20px;
    border: 3px solid #2e88b5;
}
.container1 .child1{
    width: 100%; /* 문제! */
}
.container1 .child2{
    width: 100%;
    box-sizing: border-box; /* 해결방법 1 */
}
.container1 .child3{
    width: auto; /* 해결방법 2 */
}
.container1 .child4{
    /* width의 기본 값: auto */
}
</style>

<div class="container1">
    <div class="child1"></div>
    <div class="child2"></div>
    <div class="child3"></div>
    <div class="child4"></div>
</div>
```
![스크린샷 2022-01-09 오후 9 01 10](https://user-images.githubusercontent.com/77538818/148681338-9de4135a-0eaf-4074-9065-2fe5bf4caa9d.png)

[참고 링크](https://ofcourse.kr/css-course/box-sizing-%EC%86%8D%EC%84%B1)
