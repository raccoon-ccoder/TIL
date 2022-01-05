## text-align 속성으로 정렬할 때 주의

### block 요소 내부의 inline 요소를 정렬할 때 사용하는 text-align `주의 사항`
1. `block 요소에만 text-align 속성을 적용할 수 있다.`
2. 정렬되는 것은 block 요소 안에 있는 `inline 요소만 가능`하다.
3. 2번에서 inline 요소란 text뿐만 아니라 이미지 등도 포함한다.
4. 자식 요소에게 상속된다.



### <맞는 예시>  
### div 안에 span 문자열
```html
<div>
  <span>Hello World!</span>
</div>
```
```css
div {
  text-align: center;
}
span {
  color: blue;
}
```
<img width="874" alt="스크린샷 2022-01-05 오후 11 47 13" src="https://user-images.githubusercontent.com/77538818/148237010-20de8a17-e795-45fc-b7c9-0dda443ceb81.png" />

- span의 부모인 div에게 text-align 속성을 적용하였으며, 문자열이 가로로 정렬된다.


### <틀린 예시>
### div 안에 있는 div
```html
<div class="a">
  <div class="b">Hello World!</div>
</div>
```
```css
.a {
  text-align: center;
  background-color: green;
}
.b {
  width: 300px;
  height: 300px;
  background-color: yellow;
}
```
<img width="1425" alt="스크린샷 2022-01-05 오후 11 53 56" src="https://user-images.githubusercontent.com/77538818/148238106-48c9016e-c298-420a-a7a0-fecf7e0c015b.png">
- div는 block 속성이기에 b div는 그대로이고, b div 내부에 있는 문자만 가운데 정렬이 된다.

