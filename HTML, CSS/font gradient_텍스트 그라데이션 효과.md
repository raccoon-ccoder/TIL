## `font gradient`_텍스트 그라데이션 효과 CSS 사용 방법

```css
background: linear-gradient(방향, 시작색상, 끝색상); 
webkit-background-clip: text; 
webkit-text-fill-color: transparent;
```

|background: linear-gradient;|직선 형태의 gradient 기본 형태|
|:---:|:---:|
|webkit-background-clip: text;|요소의 배경이 어디까지 차지할지 결정하며 텍스트에만 백그라운드 적용|
|webkit-text-fill-color: transparent;|텍스트 색상을 투명하게 만듦|

※ font gradient 사용시 font의 배경색을 별도로 지정하고 싶다면 `font로 지정한 태그 외부에 배경색을 위한 태그`가 존재해야 한다.

### <삽질했던 코드>
```html
<button class="home-aside__btn">
  유연한 검색
</button>
```
```css
.home-aside__btn {
    background-color: white;
    background: linear-gradient(to right, #6a008f, #9e1587);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    position: absolute;
    bottom: 150px;
    width: 145px;
    height: 60px;
    border: none;
    border-radius: 60px;
    font-size: 19px;
    font-weight: 700;
    cursor: pointer;
}
```
<img width="194" alt="스크린샷 2022-01-06 오전 1 38 45" src="https://user-images.githubusercontent.com/77538818/148254622-d101abd0-7a8c-405a-95a6-e0222593600d.png">
- 버튼의 텍스트만 보라색 그라데이션을 주고 버튼의 배경색은 흰색으로 지정하려 했으나 background 속성이 중복되어 흰색이 적용되지 못했다.   

### <수정된 코드>
```html
<button class="home-aside__btn">
  <span class="home-aside__btn-text">유연한 검색</span>
</button>
```
```css
.home-aside__btn {
    background-color: white;
    position: absolute;
    bottom: 150px;
    width: 145px;
    height: 60px;
    border: none;
    border-radius: 60px;
    font-size: 19px;
    font-weight: 700;
    cursor: pointer;
}

.home-aside__btn-text {
    background: linear-gradient(to right, #6a008f, #9e1587);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}  
```
<img width="201" alt="스크린샷 2022-01-06 오전 1 38 59" src="https://user-images.githubusercontent.com/77538818/148254662-7427b1ea-f925-4e8e-815a-6bac19a7a55a.png">
- button 내부에 텍스트를 위한 span 태그를 별도로 추가하고 span 태그에 font gradient css를 적용하였다.


