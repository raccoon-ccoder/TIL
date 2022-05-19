## [React] Warning: validateDOMNesting(...): div cannot appear as a descendant of p
리액트 프로젝트에서 만나게 된 에러다.
  
### 문제 상황
해당 에러는 리액트 개발에 사용하는 jsx에서 발생하게 되는데 p태그 안에 div를 사용했거나 p태그 안에 또 p태그를 사용했을 때 해당 에러가 발생하게 된다.   
```js
// Slider/style.ts
export const BigOverview = styled.p`
  display: block;
  color: ${(props) => props.theme.white.lighter};
  text-overflow: clip;
  overflow: hidden;
`;
 
export const BigInfo = styled.div``;
  
// Slider/index.tsx
function Slider() {
  return (
  ... 중략
   <S.BigOverview>
    <S.BigInfo>{clikedContent.overview}</S.BigInfo>
  </S.BigOverview>
  );
}
```  
  
### 해결 방안
p태그는 인라인 요소만 포함할 수 있다. div태그는 블럭 요소이므로 p태그 안에 div태그를 넣는 것은 부적절하다는 의미이다.    
해결 방법으로는 p태그를 div태그로 변경하는 방법도 있지만 컴포넌트를 분석해보니 BigInfo 요소가 굳이 필요하지 않다고 판단되어 제거하고 BigOverview만 사용했다.
