## ๐ฆ 2022-07-15 (๊ธ) 
### ์ค๋ ํ ๊ฒ
#### 1. ํ๋ก์ ํธ ๋ ์ด์์ ๊ตฌ์ฑ (ํ์ ์๋ ๊ฒ๋ค ์ ๊ฑฐ)
- metronic ํฌํ๋ฆฟ์ ์ฌ์ฉํ๋๋ฐ ๋ถํ์ํ ์์๋ค์ด ๋ง์ nav๋ฐ, header ๋ฑ ์ปดํฌ๋ํธ๋ค์ ์ ๊ฑฐํ๊ณ  toolbar์ ์ฌ์ฉ๋๋ ์์๋ค์ ์ปค์คํฐ๋ง์ด์งํ๋ค.


#### 2. React๋ก ์ฌ๋ฌ ๊ฐ์ input checkbox ๊ด๋ฆฌํ๋ ๋ฐฉ๋ฒ ํฐ๋
- ์ฌ์ฉ์๊ฐ ์ํ๋ ํญ๋ชฉ๋ง ์ ํํ๋ฉด ์ ํ๋ ํญ๋ชฉ์ ๋ํด์๋ง ์ฐจํธ๋ฅผ ๋ณด์ฌ์ค์ผ ํ๋ค.    
- ์ฌ์ฉ์๊ฐ ์ ํํ ํญ๋ชฉ์ `checkedTargets`state๋ก ๊ด๋ฆฌํ์๋ค.     
- ์ฒดํฌ ๋ฐ์ค ํด๋ฆญ์ ์ฒดํฌ๋์๋์ง ํ์ธ๊ฐ(`checked`), ํญ๋ชฉ `id`๊ฐ `onChange`ํจ์์ props๋ก ์ ๋ฌ๋๋ค.      
- ์ฒ์์๋ ๊ธฐ์กด์ ์ฒดํฌ๋์๋์ง์ ์ํ๊ฐ์ด ๋์ด์ค๋์ค ์๊ณ  checked๊ฐ `true`๋ฉด ๊ธฐ์กด์๋ ์ฒดํฌ๋์๋ค๊ฐ ์ฌ์ฉ์๊ฐ `์ฒดํฌ ํด์ `๋ฅผ ํ ๊ฒ์ด๋ผ ์ดํดํด ์ฝ๋๋ฅผ ๋ฐ๋๋ก ์งฐ์๋ค.   

#### ์ค๋ฅ ์ฝ๋
```js
const [checkedTargets, setCheckedTargets] = useState([1, 2, 3, 4, 5]);
const onChange = ({ checked, id }: IChecked) => {
    // ์ฒดํฌ๋ ์ํ -> ์ฌ์ฉ์๊ฐ ์ฒดํฌ ํด์   
    if (checked) setCheckedTargets(checkedTargets.filter(v => v !== id));
    // ์ฒดํฌ ํด์ ๋ ์ํ -> ์ฌ์ฉ์๊ฐ ์ฒดํฌํจ
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

- ํ๋ก๊ทธ๋จ์ด ์ ๋๋ก ๋์ํ์ง ์์ ์์๋ณด๋ ์ฌ์ฉ์๊ฐ ์ฒดํฌ๋ฐ์ค๋ฅผ ์กฐ์ํ๊ฒ ๋๋ฉด `๋ณ๊ฒฝ๋ ๊ฐ(checked)`์ด ๋์ด์ค๊ฒ ๋๋ค.    
- ๋ฐ๋ผ์ checked๊ฐ `true`๋ฉด `์ฒดํฌ๋ฅผ ํ ๊ฒ`์ด๋ checkedTargets ๋ฐฐ์ด์์ ํด๋น ์์ id๋ฅผ ์ถ๊ฐํ๊ณ , checked๊ฐ `false`๋ผ๋ฉด `์ฒดํฌ ํด์ `ํ ๊ฒ์ผ๋ก checkedTargets ๋ฐฐ์ด์์ ํด๋น ์์ id๋ฅผ ์ญ์ ํ๋ค.      
- ๊ธฐ๋ณธ์ธ ๊ฒ๋ ๋ชฐ๋์๋ค๋ ์ฐฝํผํ๊ธฐ๋ ํ๊ณ ... ๋ ์ด์ฌํ ๊ณต๋ถํด์ผ๊ฒ ๋ค.

#### ๊ฐ์  ์ฝ๋
```js
const [checkedTargets, setCheckedTargets] = useState([1, 2, 3, 4, 5]);
const onChange = ({ checked, id }: IChecked) => {
    // ์ฒดํฌ ํด์ ๋ ์ํ -> ์ฌ์ฉ์๊ฐ ์ฒดํฌํจ
    if (checked) setCheckedTargets([...checkedTargets, id]);
    // ์ฒดํฌ๋ ์ํ -> ์ฌ์ฉ์๊ฐ ์ฒดํฌ ํด์ ํจ
    else setCheckedTargets(checkedTargets.filter(v => v !== id));
  }
  
{targets.map((v, i) => (
  <label key={i}>
    <input type='checkbox' checked={checkedTargets.includes(v.id)} onChange={(e) => { onChange({ checked: e.currentTarget.checked, id: v.id }) }} />
    <span>{v.category}</span>
  </label>
 ))}
```



