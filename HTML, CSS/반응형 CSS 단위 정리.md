# 반응형 CSS 단위

### `절대적인 단위`
### `px`
- 모니터 위에서 화면에 나타낼 수 있는 `가장 작은 단위`
- 브라우저 사이즈 변경되어도 컨텐츠가 `고정된 값`으로 유지
- 단점 : 사용자가 브라우저에서 폰트 사이즈 설정을 변경해도 `반응하지 않는다`

### `상대적인 단위`
### `em`
- 지금 폰트 사이즈를 나타내는 단위 → 선택된 폰트 패밀리에 상관없이 항상 고정된 폰트 사이즈를 가진다
- `부모의 폰트 사이즈를 곱한 값`으로 계산된다 (부모 폰트 사이즈에서 상대적으로 크기가 계산되어진다)
```html
<div class="parent">
  Parent
  <div class="child">
    Child
  </div>
</div>
```
```css
.parent{
  font-size: 8em;
}
.child {
  font-size: 0.5em;
}
``` 
![스크린샷 2022-01-07 오후 7 54 11](https://user-images.githubusercontent.com/77538818/148533768-930e3ca0-6c95-48be-99c3-827bf3d2ab01.png)
- 따로 html이나 body에 폰트 사이즈를 지정하지 않는 이상 `기본적으로 16px` 지정
- `parent font size` : parent의 `부모 요소`인 HTML의 16px X 8 = 128px
- `child font size` : child의 `부모인 parent font size` X 0.5 = 64px

### `rem`
- r은 root를 의미하며 부모가 아닌 `root에 지정된 폰트 사이즈`에 따라서 크기가 결정된다
```html
<div class="parent">
  Parent
  <div class="child">
    Child
  </div>
</div>
```
```css
.parent{
  font-size: 8rem;
}
.child {
  font-size: 0.5rem;
}
``` 
![스크린샷 2022-01-07 오후 8 00 57](https://user-images.githubusercontent.com/77538818/148534581-f7f02cb6-eb8c-465a-b1d3-c435eed929c9.png)
- `parent font size` : `루트 요소`인, 즉 HTML에서 기본적으로 지정된 16px X 8 = 128px
- `child font size` : `최고 루트 요소`인, 즉 HTML에서 기본적으로 지정된 16px X 0.5 = 8px

### `vw`, `vh`
- 부모 요소 상관 없이 `브라우저 너비, 높이`에 따라 변경된다
- 100 vw : viewport width에 있는 100% 사용
- 50 vh : viewport height에 있는 50% 사용

### `%`
- `부모 요소`에서 상대적으로 크기가 계산된다

***


### `v*, %의 차이 1`
```html
<div class="parent">
  Parent
  <div class="child">
    Child
  </div>
</div>
```
```css
div {
  border: 1px solid black;
  padding: 32px;
}
.parent{
  font-size: 8em;
  width: 500px;
}
.child {
  background-color: burlywood;
  font-size: 0.5em;
  width: 50%;
}
```
![스크린샷 2022-01-07 오후 8 08 43](https://user-images.githubusercontent.com/77538818/148535425-8489ed5c-dbe7-44a3-871b-54aff877e4f7.png)
- child의 width는 `%`로 되어 있는데, `부모 요소인 parent`에 상대적으로 계산이 되기에 500px X 50% = 250px이 된다.

```html
<div class="parent">
  Parent
  <div class="child">
    Child
  </div>
</div>
```
```css
div {
  border: 1px solid black;
  padding: 32px;
}
.parent{
  font-size: 8em;
  width: 500px;
}
.child {
  background-color: burlywood;
  font-size: 0.5em;
  width: 50vw;
}
```
![스크린샷 2022-01-07 오후 8 10 58](https://user-images.githubusercontent.com/77538818/148535678-3bb3f8dc-c5e8-430f-95d0-7c30c813af64.png)
- child의 width는 `vw`로, 부모요소와 상관없이 `브라우저의 너비`에 따라 변경이 되기에 브라우저 너비의 절반이 된다.

### `v*, %의 차이 2`
- body의 width, height를 vw,vh / %로 표기할 때의 변화   
→ `vw, vh는 스크롤바 포함`, `%는 미포함`

```css
body {
  width: 100%;
  height: 100%;
  background-color : black;
}
```
![스크린샷 2022-01-07 오후 8 18 05](https://user-images.githubusercontent.com/77538818/148536437-128e6afa-78a4-444d-a87c-df6860ea228e.png)
- %로 표기하였을 경우는 스크롤바가 생기지 않는다

```css
body {
  width: 100vw;
  height: 100vh;
  background-color : black;
}
```
![스크린샷 2022-01-07 오후 8 18 23](https://user-images.githubusercontent.com/77538818/148536466-83332fa4-cabb-4028-9dcd-fcd89f279506.png)
- vw, vh로 표기한 경우 스크롤바가 생기게 되는데 `vw, vh`는 `스크롤바의 영역을 포함`하기 때문이다.


