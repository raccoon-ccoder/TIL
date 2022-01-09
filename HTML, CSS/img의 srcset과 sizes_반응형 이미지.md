## img의 srcset과 sizes_반응형 이미지

```html
<img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg" alt="요정 옷을 입은 엘바">
```

`srcset` - 브라우저에게 제시할 이미지 목록과 크기 정의

  1. 이미지 파일명(`elva-fairy-480w.jpg`)
  2. 공백
  3. 이미지 고유 픽셀 너비(`480w`) - px이 아니라 width인 `w`단위 사용 (이미지 실제 사이즈)

`sizes` - 미디어 조건문 및 조건문 참일시 어떤 이미지 크기가 최적인지 정의

  1. 미디어 조건문 (`(max-width: 480px)`)
  2. 공백
  3. 미디어 조건문 참일 경우 이미지가 채울 슬롯의 width (`440px`)

위처럼 정의하면 브라우저는 다음과 같이 작동한다.

  1. 기기 너비를 확인
  2. `sizes` 목록에서 가장 먼저 참이 되는 미디어 조건문 확인
  3. 해당 미디어 쿼리가 제공하는 슬롯 크키 확인
  4. 해당 슬롯 너비에 가장 근접하게 맞는 `srcset`에 연결된 이미지 불러옴

만약, 뷰포트 너비가 480px인 브라우저가 페이지를 불러올 경우라면?
  
  1. `max-width: 480px` 미디어 조건문이 참이 된다.
  2. 440px에 가장 가까운 고유 너비(`480w`)가 선택
  3. `elva-fairy-480w.jpg`가 로딩
  → 800px 사진은 128KB인데 480px 버전은 63KB이므로 65KB가 절약된다
  
## 예시 
```html
<img
  srcset="images/heropy_small.png 400w,
          images/heropy_medium.png 700w,
          images/heropy_large.png 1000w"
  src="images/heropy.png"
  alt="HEROPY" />
```
- 뷰포트 너비가 `400px 이하`일 경우, images/heropy_small.png가 사용된다.
- 뷰포트 너비가 `401 - 700px` 일 경우, images/heropy_medium.png가 사용된다.
- 뷰포트 너비가 `701px 이상`일 경우, images/heropy_large.png가 사용된다. 

## 브라우저 지원
`srcset`과 `sizes`는 IE를 지원하지 않는다.   
[참고링크](https://caniuse.com/srcset)

## 참고 자료 (Reference)
https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/
https://developer.mozilla.org/ko/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images#%ED%95%B4%EC%83%81%EB%8F%84_%EC%A0%84%ED%99%98_%EB%8B%A4%EC%96%91%ED%95%9C_%EC%82%AC%EC%9D%B4%EC%A6%88
