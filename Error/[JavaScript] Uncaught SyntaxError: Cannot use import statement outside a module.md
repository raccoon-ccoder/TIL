## [JavaScript] Uncaught SyntaxError: Cannot use import statement outside a module
바닐라 자바스크립트 토이 프로젝트(studymanager)를 진행하다 만난 오류다.

### 문제 상황
바닐라 JS로 프로젝트시 html을 잘못 연결하면 아래와 같은 오류가 발생한다.   

<br/>

![스크린샷 2022-02-22 오후 5 18 05](https://user-images.githubusercontent.com/77538818/155090951-ec3d26f7-142e-4911-84de-1eb393cd3ade.png)   
```js
// test.js
// Import the functions you need from the SDKs you need
import { initializeApp } from "https://www.gstatic.com/firebasejs/9.2.0/firebase-app.js";
```
```html
<script defer src="test.js"></script>
```
 module이 아닌 것에서 import문을 사용했다는 뜻인데, script 타입의 속성을 module로 명시하지 않아서 생긴 문제다.

<br/>

### 해결 방안
```html
<script src="test.js" type="module"></script>
```
script 태그의 type 속성값을 module로 명시한다.

#### module이란?
- JavaScript 코드의 크기가 갈수록 커지고 기능도 복잡해지자 코드 전체를 기능 단위인 코드 뭉치로 분해하고 결합하는 시스템을 마련하게 되는데 그게 바로 Module System이다.
- 즉, 코드 뭉치들이 Module이 되는 것이다.
- type이 module인 script는 defer을 붙인 것처럼 HTML 문서가 완전히 만들어진 후 실행된다.
- 따라서 모듈을 외부에서 불러올 때는 브라우저가 외부 모듈 스크립트를 병렬적으로 불러오게 된다.
