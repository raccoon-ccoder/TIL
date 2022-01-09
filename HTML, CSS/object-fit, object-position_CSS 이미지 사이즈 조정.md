## object-fit, object-position_CSS 이미지 사이즈 조정

### object-fit
img 태그나 video 태그 요소와 같은 대체 요소의 컨텐츠 `크기`를 어떤 방식으로 조절해 요소에 맞출 것인지 지정한다.   
```css
object-fit: fill;
object-fit: contain;
```
  
### object-fit 속성
|`fill`|기본값. 주어진 너비와 높이에 딱 맞도록 사이즈를 조절<br/>이미지의 가로세로 비율은 유지되지 X|
|:---:|:---:|
|`contain`|가로세로 비율을 유지한 채 사이즈 조절<br/>이미지와 컨테이너 간의 비율이 맞지 않으면 자리가 남음|
|`cover`|가로세로 비율 유지한 채 사이즈 조절<br/>비율이 안맞아도 이미지를 확대해 컨테이너 완전히 채움|
|`none`|본래의 이미지 사이즈 유지<br/>이미지 가운데가 보여지도록 맞춰짐|
|`scale-down`|none 또는 contain 중 더 적절한 방향으로 이미지 사이즈 조절|

![스크린샷 2022-01-10 오전 2 34 42](https://user-images.githubusercontent.com/77538818/148693651-399107da-5bdf-40ff-b27c-7fc6b5d85180.png)

### object-position
대체 요소의 컨텐츠 `위치`를 조정한다.
```css
/* <position> 값 */
object-position: center top;
object-position: 100px 50px;

/* 전역 값 */
object-position: inherit;
object-position: initial;
object-position: unset;
```

### object-position 속성
![다운로드](https://user-images.githubusercontent.com/77538818/148693865-087f557a-9b4a-436c-ae68-10f39e3bbda6.jpeg)

### 브라우저 호환성
`object-fit`, `object-position` 모두 IE에서는 지원이 되지 않는다.
[참고 링크](https://caniuse.com/?search=object-position)

### 참고 자료(Reference)
https://developer.mozilla.org/ko/docs/Web/CSS/object-fit
https://blogpack.tistory.com/853
