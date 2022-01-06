## box-shadow_그림자 효과

```css
  /* box-shadow: x-position y-position blur spread color; */
  box-shadow: 10px 10px 0px 5px blue;
```
- `x-position` : 정해진 수치만큼의 크기로 `오른쪽`에 그림자를 지정한다 (음수값은 왼쪽에 그림자를 지정)
- `y-position` : `아래쪽`에 정해진 수치만큼의 크기로 그림자를 지정한다 (음수값은 위쪽에 그림자를 지정)
- `blur` : 정해진 수치만큼 그림자가 `흐려지는 정도` (생략 가능, 숫자가 클수록 흐려짐)
- `spread` : 그림자의 `확산 거리`를 나타낸다 (양수면 그림자가 확대, 음수면 그림자 크기 줄어듬)
- `color` : 그림자의 `색상` 지정

### <그림지 효과 예시 1 - `일반적인 그림자`>
```html
  <div></div>
```
```css
div {
  width: 200px;
  height: 200px;
  background-color: gray;
  margin: 30px;
  box-shadow: 10px 10px 0px 5px blue;
}
```
![스크린샷 2022-01-06 오전 2 02 57](https://user-images.githubusercontent.com/77538818/148349388-e65f96d3-d4be-4caf-8afe-df91082fc8a5.png)
- x, y 값이 양수이므로 div 기준 오른쪽, 아래쪽에 그림자가 생성되었으며 `spread`는 5px이므로 5px만큼 `그림자가 커진다`고 생각하면 된다

### <그림지 효과 예시 2 - `4면 그림자`>
```html
  <div></div>
```
```css
div {
  width: 200px;
  height: 200px;
  background-color: gray;
  margin: 30px;
  box-shadow: 10px 10px 0px 5px blue, -10px -10px 5px 0px black;
}
```
![스크린샷 2022-01-06 오전 2 02 28](https://user-images.githubusercontent.com/77538818/148349397-14212757-8aea-472a-b4e5-164f9ed2b45e.png)
- 4면에 그림자를 줄 경우 box-shadow 속성에 여러 속성값을 지정하면 된다.

### <그림지 효과 예시 3 -`한쪽에만` 그림자 효과>
```css
div {
  width: 200px;
  height: 200px;
  background-color: white;
  border: 1px solid black;
  margin: 30px;
  box-shadow: 0px 10px 10px -10px black;
}
```
![스크린샷 2022-01-06 오후 5 07 56](https://user-images.githubusercontent.com/77538818/148350210-ce9c5a46-8f4a-40f3-a8cc-62bd11b30c0c.png)
- spread(그림자 확산 거리)를 음수로 설정하면 box 크기보다 작아지므로 보이지 않게 되므로
- 그런데 위와 같은 css는 그림자 효과 크기가 살짝 작으므로 ::after 등의 가상 선택자를 사용해 shadow만을 위한 박스를 만드는 것이 좋다  
