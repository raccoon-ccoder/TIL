## 🦝 2022-07-15 (금) 
### 오늘 한 것
#### 1. 프로젝트 레이아웃 구성 (필요 없는 것들 제거)
- metronic 탬플릿을 사용하는데 불필요한 요소들이 많아 nav바, header 등 컴포넌트들을 제거하고 toolbar에 사용되는 요소들을 커스터마이징했다.


#### 2. React로 여러 개의 input checkbox 관리하는 방법 터득
- 사용자가 원하는 항목만 선택하면 선택된 항목에 대해서만 차트를 보여줘야 한다.    
- 사용자가 선택한 항목은 `checkedTargets`state로 관리하였다.     
- 체크 박스 클릭시 체크되었는지 확인값(`checked`), 항목 `id`가 `onChange`함수의 props로 전달된다.      
- 처음에는 기존에 체크되었는지의 상태값이 넘어오는줄 알고 checked가 `true`면 기존에는 체크되었다가 사용자가 `체크 해제`를 한 것이라 이해해 코드를 반대로 짰었다.   

#### 오류 코드
```js
const [checkedTargets, setCheckedTargets] = useState([1, 2, 3, 4, 5]);
const onChange = ({ checked, id }: IChecked) => {
    // 체크된 상태 -> 사용자가 체크 해제  
    if (checked) setCheckedTargets(checkedTargets.filter(v => v !== id));
    // 체크 해제된 상태 -> 사용자가 체크함
    else setCheckedTargets([...checkedTargets, id]);
  }
  
{targets.map((v, i) => (
  <label key={i}>
    <input type='checkbox' checked={checkedTargets.includes(v.id)} onChange={(e) => { onChange({ checked: e.currentTarget.checked, id: v.id }) }} />
    <span>{v.category}</span>
  </label>
 ))}
```

<br />

- 프로그램이 제대로 동작하지 않아 알아보니 사용자가 체크박스를 조작하게 되면 `변경된 값(checked)`이 넘어오게 된다.    
- 따라서 checked가 `true`면 `체크를 한 것`이니 checkedTargets 배열에서 해당 요소 id를 추가하고, checked가 `false`라면 `체크 해제`한 것으로 checkedTargets 배열에서 해당 요소 id를 삭제한다.      
- 기본인 것도 몰랐었다니 창피하기도 하고... 더 열심히 공부해야겠다.

#### 개선 코드
```js
const [checkedTargets, setCheckedTargets] = useState([1, 2, 3, 4, 5]);
const onChange = ({ checked, id }: IChecked) => {
    // 체크 해제된 상태 -> 사용자가 체크함
    if (checked) setCheckedTargets([...checkedTargets, id]);
    // 체크된 상태 -> 사용자가 체크 해제함
    else setCheckedTargets(checkedTargets.filter(v => v !== id));
  }
  
{targets.map((v, i) => (
  <label key={i}>
    <input type='checkbox' checked={checkedTargets.includes(v.id)} onChange={(e) => { onChange({ checked: e.currentTarget.checked, id: v.id }) }} />
    <span>{v.category}</span>
  </label>
 ))}
```



