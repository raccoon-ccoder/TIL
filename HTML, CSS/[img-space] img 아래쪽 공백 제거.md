## [img-space] img 아래쪽 공백 제거
### img의 공백
`<img>` 태그 사용시 기본적으로 아래쪽에 공백이 생긴다.
```html
<div class="container">
        <img src="img/cat.jpeg">
</div>
```
```css
.container {
  width: 300px;
  border: 1px solid black;
}
img {
  max-width: 100%;
}
```
<img width="332" alt="스크린샷 2022-02-17 오후 5 59 09" src="https://user-images.githubusercontent.com/77538818/154441051-bae5c424-d568-4229-ab76-19180d2f57e4.png">

### 공백이 생기는 이유
`<img>`태그는 인라인-레벨 요소이기 때문에 기본적으로 일반 텍스트와 동일하게 baseline 기준으로 정렬된다. 즉, `vertical-align : baseline`이 기본값이라는 말이다.   

<br/>

![img-baseline](https://user-images.githubusercontent.com/77538818/154441411-4d4b89db-e90c-489a-b5f9-d8f9f4161145.gif).  

<br/>

위 그림에서 텍스트가 baseline 기준으로 정렬된 것을 볼 수 있는데 `<img>`도 마찬가지다. baseline 기준으로 정렬된 인라인-레벨 요소는 알파벳의 y, j, p, g, q 등과 같은 하강문자(desender)를 위해 
더 공간이 있어야 하기 때문에 `<img>`의 아래쪽에 공백이 생기는 것이다.

### 해결방법
- `<img>`에 `display:block`
  - 블록-레벨 요소로 바꿔서 해결하는 방법인데 이러면 한 줄을 다 차지한다는 단점이 있다.
- 컨테이너에 `line-height:0`
  - 컨테이너에 포함된 인라인 요소의 `line-height`를 없애는 방법인데, 그 안에 포함된 모든 텍스트의 `line-height`에 영향을 주기에 좋은 방법은 아니다.
- `<img>`에 `vertical-align: middle/top/bottom`
  - `vertical-align`을 바꾸는 방법으로 baseline의 경우에만 아래쪽 공간이 생기니 기준을 바꿔버리는 것이다. **가장 좋은 방법이다.**
