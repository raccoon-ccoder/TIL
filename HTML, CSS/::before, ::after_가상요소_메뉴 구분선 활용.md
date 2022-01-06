## ::before, ::after_가상요소_메뉴 구분선 활용

### `::before`
- 선택한 요소의 `첫번째 자식`으로 생성되는 가상 요소로 content 속성은 필수며 기본값은 인라인이다.

### `::after`
- 선택한 요소의 `맨 마지막 자식`으로 생성되는 가상 요소로 content 속성은 필수며 기본값은 인라인이다.

### <::before 예시 1 - `메뉴 구분선`>
```html
<ul>
  <li>login</li>
  <li>home</li>
  <li>settings</li>
</ul>
```
```css
li {
  display: inline-block;
  margin-left: 10px;
}

li::after {
  content: "|";
  padding-left: 10px;
}

li:last-child::after {
  content: none;
}
```
![스크린샷 2022-01-06 오후 5 28 34](https://user-images.githubusercontent.com/77538818/148352886-cb77354a-f1d5-43f8-b756-f6bd4322b993.png)
- li 태그에 ::after 가상 요소를 설정하여 li 태그 내부의 마지막에 가짜 자식 요소인 구분선을 추가하였고 마지막 li 태그에는 구분선을 없애주기 위해 content: none을 설정하였다

### <::before 예시 2 - `원하는 높이의 세로 메뉴 구분선`>

```html
<div class="search-form__input search-form__place">
  <span class="search-form__title">위치</span>
  <input type="text" placeholder="어디로 여행가세요?" />
</div>

<div class="search-form__input search-form__check-in">
  <span class="search-form__title">체크인</span>
  <input type="text" placeholder="날짜 입력"/>
</div>

<div class="search-form__input search-form__check-out">
  <span class="search-form__title">체크아웃</span>
  <input type="text" placeholder="날짜 입력" />
</div>

<div class="search-form__input search-form__number">
  <span class="search-form__title">인원</span>
  <input type="text" placeholder="게스트 추가" />
  <button class="search-form__number-btn">
    <i class="fas fa-search fa-lg"></i>
  </button>
</div>
```
```css
.search-form__input::before {
    content: "";
    position: absolute;
    top: 18px;
    left: 0;
    height: 35px;
    border-left: 0.5px solid var(--little-gray);
}

.search-form__input:first-child::before {
    content: none;
}
```
![스크린샷 2022-01-06 오후 5 28 46](https://user-images.githubusercontent.com/77538818/148352922-c966f187-ec01-4748-be31-0053b30f625f.png)
- content: "|"로 설정하면 길이를 따로 설정할 수 없어 각 div 태그 내부에 첫번째 자식으로 가상 박스를 생성하였고 박스의 border를 생성하여 세로 구분선을 만들었다.

### <::after 예시 1 - `a태그 hover시 밑줄 트랜지션`>
```html
<li class="welcome-header__item">     
  <a href="#" class="welcome-header__a">숙소</a>
</li>
<li class="welcome-header__item">
  <a href="#" class="welcome-header__a">체험</a>
</li>
<li class="welcome-header__item">
  <a href="#" class="welcome-header__a">온라인 체험</a>
</li>
```
```css
.welcome-header__a::after {
    display: block;
    content: '';
    border-bottom: 2px solid white;
    transform: scaleX(0);
    transition: transform 250ms ease-in-out;
}

.welcome-header__a:hover {
    color: var(--little-gray); 
}

.welcome-header__a:hover::after {
    transform: scaleX(1);       
}
```
![스크린샷 2022-01-06 오후 8 17 35](https://user-images.githubusercontent.com/77538818/148374932-e7387d25-1c81-42ff-9e9c-15ac6e1788b3.png)
- ::after, 트랜지션을 이용하여 평상시엔 밑줄이 없다가 hover시 a태그 밑쪽에 밑줄이 생기는 효과를 주었다

### 브라우저 호환성
- 22.01.06 기준 IE, Safari 지원 불가 
