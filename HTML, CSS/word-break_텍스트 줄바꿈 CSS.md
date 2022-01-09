## word-break_텍스트 줄바꿈 CSS

### word-break
- 텍스트가 컨텐츠 박스 밖으로 overflow 했을 때 `줄을 어떻게 바꿀지` 지정한다.

### word-break 속성값의 중단점
|  |`normal`(기본)|`break-all`|`keep-all`(CJK에만 적용)|
|:---:|:---:|:---:|:---:|
|CJK|음절|음절|공백(띄어쓰기, 하이픈)|
|non-CJK|공백(띄어쓰기, 하이픈)|음절|공백(띄어쓰기, 하이픈)|

```html
<p>1. <code>word-break: normal</code></p>
<p class="normal narrow">This is a long and
 Honorificabilitudinitatibus califragilisticexpialidocious Taumatawhakatangihangakoauauotamateaturipukakapikimaungahoronukupokaiwhenuakitanatahu
 안녕하세요 반갑습니다 세상에서 제일 가장 단어는 니코틴아마이드 아데닌 다이뉴클레오타이드이며 클로로트리플루오로에틸렌중합체도 있습니다</p>

<p>2. <code>word-break: break-all</code></p>
<p class="breakAll narrow">This is a long and
 Honorificabilitudinitatibus califragilisticexpialidocious Taumatawhakatangihangakoauauotamateaturipukakapikimaungahoronukupokaiwhenuakitanatahu
 안녕하세요 반갑습니다 세상에서 제일 가장 단어는 니코틴아마이드 아데닌 다이뉴클레오타이드이며 클로로트리플루오로에틸렌중합체도 있습니다</p>

<p>3. <code>word-break: keep-all</code></p>
<p class="keepAll narrow">This is a long and
 Honorificabilitudinitatibus califragilisticexpialidocious Taumatawhakatangihangakoauauotamateaturipukakapikimaungahoronukupokaiwhenuakitanatahu
 안녕하세요 반갑습니다 세상에서 제일 가장 단어는 니코틴아마이드 아데닌 다이뉴클레오타이드이며 클로로트리플루오로에틸렌중합체도 있습니다</p>
```
```css
.narrow {
  padding: 5px;
  border: 1px solid;
  display: table;
  max-width: 100%;
}

.normal {
  word-break: normal;
}

.breakAll {
  word-break: break-all;
}

.keepAll {
  word-break: keep-all;
}
```
![스크린샷 2022-01-10 오전 1 27 56](https://user-images.githubusercontent.com/77538818/148691251-71290ebd-4850-4dba-8c16-580afdadc680.png)
