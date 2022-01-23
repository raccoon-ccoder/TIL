## [짧은 CSS팁] 쉬운 중앙 정렬

### 1. position, transform 사용
```css
h1 {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```
- `position: absolute` - 부모 엘리먼트 위치를 기준으로 절대적인 위치 값을 설정할 수 있다.
- `left, top: 50%` - 중앙에 정렬한다. (이렇게 하면 대상 엘리먼트의 `좌측 상단이 중앙`에 오게된다)
- `transform: translate(-50%, -50%)` - 대상 엘리먼트 크기의 절반만큼 빼서(-) x축, y축을 이동시켜 엘리먼트가 정중앙에 오게 한다.

### 2. flex 사용
```css
.flex {
  display: flex;
  justify-contents: center;
  align-items: center;
}
```
- `flex`를 사용하게 되면 `position: absolute`를 쓰기 어려운 환경에서 적절하다. (해당 엘리먼트가 아닌 부모 엘리먼트에 적용해야 한다)
- div 안의 텍스트를 정중앙에 위치시킬 때, 버튼의 이미지를 정중앙에 위치시킬 때 등 절대 위치를 적용하기 힘든 모든 상황에서 적절하게 사용할 수 있다.   
  
### 3. vertical-align 사용
```css
.middle{
  vertical-align: middle;
}
```
- 보통은 1, 2번으로 처리가 가능하지만 `display: inline-block`을 사용하고 `flex를 절대 쓰고 싶지 않은 경우` 위와 같은 방법을 사용해도 된다.
- `vertical-align`은 동일한 레벨에 있는 다른 엘리먼트의 높이에 영향을 받고, 부모 엘리먼트의 높이가 변할 때 따라서 반응하지 않는다.
- [참고링크](https://codepen.io/joshephan/pen/ExyrEyW)
 
### 4. line-height 사용
```css
.icon-button {
    height: 48px;
    width: 48px;
    text-align: center;
}

.icon-button > span.icon {
    line-height: 48px;  /* 부모 엘리먼트의 높이, 잘 안 맞는 경우 1px씩 조절 */
}
```
- 폰트 기반의 아이콘을 중앙에 위치시킬 경우 간단하게 쓸 수 있는 방법이다.
- 아이콘을 `span`으로 감싼 후 `line-height` 값을 직접 입력한다.
- 보통은 부모 엘리먼트 높이를 그대로 입력하면 중앙에 오는데, 폰트 기반의 아이콘의 경우 살짝 안 맞는 경우가 있기에 1px씩 조절해 맞춰준다.

### 5. margin: auto 사용
```css
body {
    width: 100%;
    padding: 0;
    margin: 0;
}

main {
    width: 100%;
    max-width: 1000px;
    margin: auto;
}
```
- 보통 `메인 컨텐츠 컨테이너를 수평 중앙`에 둘 때 사용한다. 좌우 여백이 최대폭을 기준으로 반반씩 정확히 가져갈 수 있게 한다.
- `display: inline, inline-block`이면 제대로 작동하지 않는다. (inline은 영역을 차지하지 않고, inline-block은 컨텐츠가 있는 부분만 영역처리를 하기에 margin이 제대로 적용되지 않음)

### 6. text-align: center 사용
```css
.center {
    text-align: center;
}
```
- 엘리먼트 안의 구성요소가 텍스트, inline 또는 inline 계열일 때 수평 중앙에 두는 방법이다.
- `margin: auto`와 마찬가지로 수평 중앙만 맞출 수 있다.

### 7. Grid와 place-content 사용
```css
.container {
    display: grid;
    place-content: center;
}
```

