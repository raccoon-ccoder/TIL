## [CSS Module] React Component 스타일링
- CSS를 불러와서 사용할 때 클래스 이름을 고유한 값으로 자동으로 만들어서 컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지하는 기술이다.   
- 확장자는 `.module.css` 파일로 저장하면 CSS Module이 적용된다.    
- CSS Module이 적용된 스타일 파일을 불러오면 JS 객체를 하나 전달받게 되는데 CSS Module에서 사용된 클래스명과 해당 이름을 고유화한 `key-value` 형태로 들어있다.  
- 따라서 CSS Module이 적용된 웹사이트를 개발자 도구에서 확인해보면 특이한 형태로 클래스명이 부여된 것을 볼 수 있다.     
- 아래의 사진을 보면 `컴포넌트명_스타일 클래스명_고유한 클래스명`으로 부여되기에 여러 컴포넌트의 CSS 파일에 같은 클래스명이 있어도 겹쳐서 충돌하는 문제는 없다.   
<img width="626" alt="스크린샷 2022-03-16 오후 9 04 00" src="https://user-images.githubusercontent.com/77538818/158586191-5ff43f62-10fe-4952-8e57-0a80c2f2b9f3.png">

<br/>

### className에 하이픈(-)이 포함된 경우
```js
import React from 'react'
import style from './style.module.css';
 
const Home = () => {
    return(
        <header className={style['header-image']}>
            <span>하이픈 포함되어 배열로 스타일링</span>
        </div>
    );
};
 
export default Home;
``` 
고유한 클래스명을 사용하려면 클래스를 적용하고 싶은 JSX Element에 className={style['클래스-이름']} 배열로 감싼 형태로 전달하면 된다.

### className에 하이픈이 없는 경우
```js
import React from 'react'
import style from './style.module.css';
 
const Home = () => {
    return(
        <div className={style.header}>
            <span>하이픈 미포함되어 배열로 스타일링</span>
        </div>
    );
};
 
export default Home;
```
클래스를 적용하고 싶은 JSX Element에 className={style.클래스이름} 형태로 전달해 주면 된다.


### CSS Module을 사용한 클래스 이름을 두 개 이상 적용할 때
```js
import React from 'react'
import style from './style.module.css';
 
const Home = () => {
    return(
        <div className={`${style.header} ${style.nav}`}>
            <span>Class 이름을 두개 이상 사용할 </span>
        </div>
    );
};
 
export default Home;
```
위 코드에선 ES6 문법 템플릿 리터럴을 사용해 문자열을 합해 주었다.   
템플릿 리터럴 방법이 싫다면 다음과 같이 작성할 수 있다.
```js
className={[style.header, style.nav].join(' ')}
```






