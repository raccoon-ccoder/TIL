- :first-child, :last-child  선택자 사용 이유?
	- 의사 클래스 선택자 (가상 클래스 느낌) 중 한 종류로 기존에 존재한 선택자로 구체적인 스타일을 줄 수 없기에 가상 클래스, 가상 요소로 구체적으로 스타일을 나타낼 수 있음

- 요소:first-child { 속성: 속성값; } 
	- 부모 요소 안에 있는 첫번째 자식만을 선택할 수 있는 선택자
	
```html
<h4>커피 메뉴</h4>
<div>
	<p>아메리카노</p>
	<p>카페라떼</p>
	<p>카푸치노</p>
</div>

<h5>옵션 선택</h5>
<ol>
	<li>샷 추가</li>
	<li>얼음 추가</li>
	<li>우유 변경</li>
</ol>
```

```css
p:first-child{  color:green;}
ol  >  li:last-child{background-color:  blue;}
```

<img width="242" alt="스크린샷 2021-12-27 오후 6 23 05" src="https://user-images.githubusercontent.com/77538818/147456984-2f67ce43-811a-4292-a216-710e341a1d93.png">
