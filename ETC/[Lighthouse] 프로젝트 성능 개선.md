## [Lighthouse] 프로젝트 성능 개선
바닐라 자바스크립트로 토이 프로젝트를 진행하며 Lighthouse로 프로젝트 성능을 개선하며 배운 것들을 정리한 글이다.

### Lighthouse란?
웹앱 품질 측정도구로 웹앱의 성능, 접근성, SEO 등을 검사할 수 있으며 크롬 개발자 도구에서 Lighthouse 탭으로 들어가 확인할 수 있다.  
배포 환경에서 성능을 측정한 결과 

<br/>

![light house 전](https://user-images.githubusercontent.com/77538818/156950687-b29a17b4-6a5a-4867-adea-441dbf8cd0b0.png)

<br/>
노란 불이 세 개나 켜졌기에 라이트 하우스에서 어떤 부분을 개선시키면 되는지 확인하여 수정하였다.

## Performance
### Eliminate render-blocking resources
- 브라우저가 다운로드하고 해석하는 동안 웹페이지에는 아무것도 보이지 않기 때문에 이런 요인을 제거해야 한다는 경고
- 즉, 브라우저가 외부 리소스를 다운로드하고 파싱하는 동안 페이지 콘텐츠를 파싱하거나 렌더링하지 않기 때문에 페이지 표시 속도 저하의 원인이 된다.

>렌더링 블로킹 리소스 표시 조건
>- scirpt 태그의 경우    
>   - defer, async 속성이 없는 것
>   - 문서의 head 태그 안에 있는 것
>- link rel="stylesheet"의 경우
>   - disabled 속성이 없는 것 (disabled 속성이 지정되어 있으면 브라우저는 그 파일을 다운로드 하지 않음)
>   - 유저의 기기에 맞는 media 속성을 가지지 않은 것

<br/>

### 어떻게 필수적인 부분을 찾을 수 있을까?
크롬 개발자 도구의 coverage tab에서 검사하여 실제로 어느 부분이 사용되는지 확인 할 수 있다.
- 초록색 부분 : first paint에 필수적인 스타일 혹은 페이지의 핵심 기능에 관련된 코드
- 빨간색 부분 : 즉시 보이지 않는 컨텐츠에 적용되는 스타일 혹은 페이지의 핵심 기능에 사용되지 않는 코드

<br/>

![스크린샷 2022-03-07 오전 10 15 29](https://user-images.githubusercontent.com/77538818/156951739-b0aff8c3-7839-490b-a979-d657fc93047f.png)

<br/>

해결방법으로는 다음과 같이 defer, async 속성을 이용하면 된다.  
참고로 defer은 웹페이지가 모두 그려지고 'DOM'이 들어왔을 때, 그 때 스크립트를 실행하기에 가장 추천하는 옵션이다.
```html
//병렬 다운로드, 즉시 실행
  <script async src="script.js"></script>
//병렬 다운로드, 지연 실행
  <script defer src="script.js"></script>
```

__따라서, script 태그의 경우 다음과 같이 작성하는 것이 좋다.__
1. 필수 스크립트는 html에 `<script>`형식으로 작성.
2. 기타 스크립트는 `<body>` 종료 태그 직전에 선언.
3. 마지막에 파싱해도 문제 없으면 `defer` 속성.
4. 가능한 빠른 시점에 실행 필요하면 `async` 속성.

__link 태그의 경우 다음과 같이 작성하는 것이 좋다.__
1. 반응형 웹인 경우 해상도 구간별로 CSS 파일을 분리해 `media` 속성으로 분기
```html
 <link href="*.css" rel="stylesheet" media="(max-width: 639px)">
 <link href="*.css" rel="stylesheet" media="(min-width: 640px) and (max-width:960px)">
 <link href="*.css" rel="stylesheet" media="(min-width: 961px)">
```
2. 필수 스타일 요소는 `head` 태그 내부에 style 태그 형식으로 작성
3. 나머지 스타일 요소는 link 태그에 `preload`를 사용해 비동기적으로 불러오기
```html
 <link rel="preload" as="style" href="*.css" onload="this.rel='stylesheet'">
```

<br/>

### Ensure text remains visible during webfont load
CSS의 `@font-face` 부분에 `font-display: swap`를 넣어주면 폰트가 로딩되지 않았을 때나 폰트를 로드하지 못했을 때 시스템 폰트를 보여준다. 
이렇게 넣어주지 않으면 폰트가 로드되지 않았을 때 글씨가 화면에 나타나지 않는다.

<br/>

link 태그로 불러오는 웹 폰트일 경우,  `<link rel="preload" as="font">` 처럼 `preload`를 넣어 주면 폰트를 먼저 불러오기 때문에 렌더링된 처음부터 폰트가 적용되어 있다.

<br/>

만약 구글 폰트라면, url 뒤에 `&display=swap` 을 넣어주면 swap이 적용된다. 예시: `<link href="https://fonts.googleapis.com/css?family=Roboto:400,700&display=swap" rel="stylesheet">`

<br/>

## Accessibility
### Background and foreground colors do not have a sufficient contrast ratio
`color` 값과 `background-color`값의 대비가 충분하지 않아 발생한 경고로, 개발자 도구 창에서 해당 요소의 styles 탭에서 color의 색상 미리보기 사각형을 클릭하면 contrast ratio를 확인할 수 있다.
- `AA` : 최소 조건 대비. 텍스트의 배경과 텍스트의 대비는 4.5:1이어야 한다. 단, 텍스트의 크기가 큰 경우에는 3:1의 대비만 충족되어도 된다. 텍스트가 단순히 페이지를 꾸미기 위한 용도이거나 로고나 브랜드 이름의 일부일 경우에는 대비 조건을 만족하지 않아도 된다.
- `AAA` : 최적 조건 대비. 배경과 텍스트가 7:1의 대비를 가져야 좋다. 단, 텍스트의 크기가 클 경우에는 4.5:1의 대비만 가져도 된다. AA와 마찬가지로, 단순 꾸미기 위한 텍스트나 로고나 브랜드 이름의 일부가 되는 텍스트는 대비 조건을 만족하지 않아도 된다.    

<br/>

![접근성 - 배경, 요소 배경화면 명암비 조정](https://user-images.githubusercontent.com/77538818/156963432-060402ae-be2f-47de-a88a-e4fca0d24acb.png)

<br/>

### Form elements do not have associated labels
`input`태그에 `label`태그가 연결되어 있지 않다는 경고로 label 태그를 추가 후 연결해주면 된다.
```html
// 방법 1
<label>
  Receive promotional offers?
  <input type="checkbox">
</label>

// 방법 2
<input id="promo" type="checkbox">
<label for="promo">Receive promotional offers?</label>
```

<br/>

## SEO
### Document does not have a meta description
해당 페이지와 관련된 정보를 담는 `meta description`태그가 존재하지 않는다는 경고다. 따라서 head 태그 내부에 추가하면 된다. 토이 프로젝트에서는 작성자 정보와 키워드 태그도 추가하였다.   
참고로 og 태그들을 작성했다고 해서 SEO 라고 오해하면 안된다.

```html
<meta name="keywords" content="study, timer, timetrack, planner">
<meta name="author" content="jybaek96">
<meta name="description" content="타이머로 공부시간을 기록하자, studymanager">
```

<br/>

## 최종 개선 결과
![프로젝트 성능 개선 후](https://user-images.githubusercontent.com/77538818/156964390-382a6ea8-a8aa-4501-a5a6-6198ea069b28.png)

위와 같이 성능 개선 후 lighthouse로 다시 측정해보니 그래도 많이 개선되었지만 100점은 아니라서 좀 아쉬운 부분도 있다. 리팩토링을 하며 더 개선하고 싶다.

### 참고 자료(Reference)
[Performance - Eliminate render-blocking resources 관련 방법](https://web.dev/defer-non-critical-css/)     
[Performance - render-blocking](https://web.dev/render-blocking-resources/)     
[Accessibility - contrast ratio 관련](https://web.dev/color-contrast/)      
[SEO - meta description](https://web.dev/meta-description/)
