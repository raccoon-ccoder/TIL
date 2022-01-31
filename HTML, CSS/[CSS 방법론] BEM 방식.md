## [CSS 방법론] BEM 방식
CSS 클래스명을 효율적으로 짓기 위한 방법론 중 하나로 모멘텀 클론 코딩 프로젝트에 BEM 방식을 적용하였다.

### BEM이란?
- `Block`, `Element`, `Modifier`로 구성되어 있으며 `__`와 `--`로 구분한다.
- 기본적으로 ID를 사용하지 않으며, class만을 사용한다.
- '어떻게 보이는가'가 아닌 `'어떤 목적인가'`에 따라 이름을 짓는다.
- 예를 들어, 에러 메시지를 띄우는 p태그는 `.red`가 아닌 `.error`라는 이름으로 지어야 한다.
- 이름을 연결할 때는 `block-name`과 같이 하이픈 하나만 써서 연결한다.
```css
.header__navigation--navi-text {
  color: red;
}
```
- 위 코드에서 header는 Block, navigation은 Element, navi-text는 Modifier가 된다.

### Block, Element, Modifier
![스크린샷 2022-01-31 오후 8 59 55](https://user-images.githubusercontent.com/77538818/151790278-59118433-c4fb-4eab-af5d-9cf2d0d96ffa.png)
#### Block 
- **재사용 가능한 기능적으로 독립적인 페이지 컴포넌트**를 의미한다. 
- 위 깃허브 사진에선 logo 블럭 같이 어딘가에 종속되지 않고 헤더에 쓰거나, 푸터에 쓸 수도 있는 재사용 가능한 블럭을 의미한다
- 또한 블럭은 블럭을 감쌀 수 있다. logo 블럭은 header에 위치하므로 `.header<.logo`로 표현할 수 있다.

#### Element
- `블럭을 구성하는 단위`다.
- 블럭은 독립적이나, 엘리먼트는 의존적인 형태로 `자신이 속한 블럭 내에서만 의미를 가지기에` 블럭 안에서 떼어다 다른 곳에 쓸 수 없다.
- 위 사진에서 Features, Blog 같이 menu 블럭 내의 menu elements들이 예로 있다.
```html
<form class="search-form">
    <input class="search-form__input"/>
    <button class="search-form__button">Search</button>
</form>
```
- 위 코드에서 `.search-form`은 블럭이고, `.search-form__input`과 `.search-form__button`은 엘리먼트이다.
- search-form 블럭은 재사용 가능하지만, 내부의 input과 button은 검색을 위한 인풋창과 버튼이기에 search-form 안에서만 존재 의미가 있다.
- 엘리먼트도 중첩이 가능하다. (`.block > .block__element1 > .block__element2`)
- BEM의 포인트 중 하나는 `block__element2`를 `block__element1`의 하위 엘리먼트로 보지 않고 둘 다 `.block`의 엘리먼트로 취급한다.

#### Modifier
- `블럭이나 엘리먼트의 속성`을 담당한다.   
- 깃허브 사진에선 input의 size big, button의 theme green이 modifier에 해당한다.   

```html
<ul class="tab">
  <li class="tab__item tab__item--focused">탭 01</li>
  <li class="tab__item">탭 02</li>
  <li class="tab__item">탭 03</li>
</ul>
```
- 위 코드에선 `--focused`가 modifier이며 저런 방식은 `boolean 타입`이며 그 값이 true라고 가정하고 사용한다.   

```html
<div class="column">
  <strong class="title">일반 로그인</strong>
  <form class="form-login form-login--theme-normal">
    <input type="text" class="form-login__id"/>
    <input type="password" class="form-login__password"/>
  </form>
</div>
 
<div class="column">
  <strong class="title title--color-gray">VIP 로그인 (준비중)</strong>
  <form class="form-login form-login--theme-special form-login--disabled">
      <input type="text" class="form-login__id"/>
      <input type="password" class="form-login__password"/>
  </form>
</div>
```
- 성질-내용을 작성하고 싶을 경우 `key-value` 형태로 `color-gray`, `theme-normal`과 같이 표현할 수 있다.

### 코드 예제
#### 가장 큰 블럭 나누기
![스크린샷 2022-01-31 오후 9 13 27](https://user-images.githubusercontent.com/77538818/151791961-121270c5-aabf-4716-8905-5c0cf806fd77.png)
```html
<div class="header">
  <div class="header__inner">
    <div class="tabzilla"></div>
    <div class="header__logo"></div>
    <div class="header__auth"></div>
    <div class="nav"></div>
    <div class="header__search"></div>
  </div>
</div>
```
- 위 코드에서 `.header는 블럭`이며, `.header__`로 시작하는 태그들은 모두 엘리먼트에 해당한다.
- `.tabzilla`와 `.nav`는 엘리먼트가 아닌 블럭으로, 다른 곳에서도 독립적으로 쓰일 수 있기 때문에 블럭으로 지정한 것으로 보인다.

#### 로고 아래 링크 넣기
```html
<div class="header__logo">
  <div class="logo">
    <a href="#" class="logo__link">
      <h1 class="blind">MDN</h1>
    </a>
  </div>
</div>
```
- `.header__logo`라는 요소 아래 logo라는 블럭이 있는데 `.logo` 역시 별도로 쓰일 수 있기에 위와 같이 작명한듯하다.
- `.header__logo`는 **헤더 내에서 로고 위치를 잡는 데 쓰고**, `.logo`는 **로고 이미지를 지정**하는 데 사용하면 된다.
- 이 로고를 푸터에도 써야 한다면, `.footer__logo > .logo`로 마크업하고 `.footer__logo`에 css로 위치만 잡아주면 된다.

#### Sign in with 부분 작성하기
```html
<div class="header__auth">
  <div class="auth">
    <div class="auth-means"></div>
    <div class="auth-picker"></div>
  </div>
</div>
```
- 이 부분도 auth라는 블럭으로 떼어냈다.
- 마우스 오버시 아래 리스트가 나오기에 auth를 위/아래로 나눴으며 __ 를 써서 요소로 나눈 게 아니라 -를 이용해 블럭으로 나눴다.  

```html
<div class="auth-means">
  <p class="auth-means__text">Sign in with
  <ul class="auth-means__list">
    <li class="auth-means__item auth-menas__item--persona">
      <a href="#">
        <i class="fa fa-user"></i>
        <span class="blind">Persona</span>
      </a>
    </li>
    <li class="auth-means__item auth-menas__item--github">
      <a href="#">
        <i class="fa fa-github"></i>
        <span class="blind">Github</span>
      </a>
    </li>
  </ul>
</div>
```
- auth-mean 부분은 텍스트 / 리스트로 나눴으며 리스트 아래에는 아이템이란 이름의 엘리먼트가 있다.
- 아이콘이 2개인데 `--persona`와 `--github` 로 modifier를 쓴 이유는 따로 스타일을 지정하기 위함이다.

### 참고자료 (Reference)
[bem 소개](http://getbem.com/introduction/)    
[bem 요약 블로그](https://nykim.work/15)  
[bem by example 한글 번역](https://naradesign.github.io/bem-by-example.html)   
