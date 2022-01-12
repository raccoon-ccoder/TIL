# 백준 JavaScript VSCode 환경 세팅
백준 사이트에서 자바스크립트로 문제를 풀 경우 node.js를 선택해야 하기에 VSCode에 환경세팅을 한다.

## 1. Node.js 다운로드
- [Node.js](https://nodejs.org/en/) 사이트에 접속하여 다운로드를 진행한다.
![스크린샷 2022-01-12 오전 2 18 03](https://user-images.githubusercontent.com/77538818/148990399-7262aca9-de74-44a9-b06a-801198bae52d.png)  
- 왼쪽은 LTS버전이고 오른쪽은 Current 버전인데 학습을 위해서 최신 버전인 Current을 다운받았으며 설치 완료 후 터미널에서 Node.js가 정상적으로 다운로드 되었는지 아래의 명령어로 확인해본다.   
```
node -v
혹은
npm -v
// npm (Node Packaged Manager) : Node.js로 만들어진 pakage(module)을 관리해주는 툴
```
<img width="238" alt="스크린샷 2022-01-12 오후 12 38 35" src="https://user-images.githubusercontent.com/77538818/149059684-1c91f1e7-32be-4598-83b9-1c11b9bbcc6e.png">   

## 2. VSCode 세팅
### Code Runner 플러그인 다운로드
- Code Runner를 이용해 VSCode 내장 터미널에서 단축키를 사용해 자바스크립트를 비롯한 다양한 프로그래밍 언어로 구현된 소스코드를 간단히 실행할 수 있다.   
![스크린샷 2022-01-12 오후 12 41 37](https://user-images.githubusercontent.com/77538818/149059977-8ace8c88-f8ed-4827-a6c3-c4e4efcd8c5a.png)     
- 실행 단축키(MacOS) : `control + option + n` 혹은 터미널에서 실행하고자 하는 js파일의 상위 디렉토리로 이동 후 `node + 실행할 js파일명`   

![스크린샷 2022-01-12 오후 12 44 01](https://user-images.githubusercontent.com/77538818/149060165-00f9de7c-c960-40f5-9201-12a64946e7da.png)   
<img width="578" alt="스크린샷 2022-01-12 오후 12 47 11" src="https://user-images.githubusercontent.com/77538818/149060483-303fe65f-00f5-4b8e-978a-fcc001ef506a.png">     
 
 ## 3. 알고리즘 문제 풀이 과정
 [백준 1001번 문제](https://www.acmicpc.net/problem/1001)   
 ![스크린샷 2022-01-12 오후 12 50 56](https://user-images.githubusercontent.com/77538818/149060835-728e5f38-7edf-41c0-a96a-c3e86c6c1dc1.png)   
 ### (1) 예제 입력이 담길 txt 파일 - test.txt
 문제의 예제 입력을 복사해서 위 파일에 붙여넣기한다.   
 
 <img width="317" alt="스크린샷 2022-01-12 오후 12 52 25" src="https://user-images.githubusercontent.com/77538818/149060974-08e2c4dc-3ced-48ff-905b-290944704177.png">   
 
 ### (2) 실행 JS 파일 - index.js
 test.txt의 입력 내용을 받아서 실행시키는 파일이다.
```node.js
let input = require('fs').readFileSync('test.txt').toString().split(' ');
var a = parseInt(input[0]);
var b = parseInt(input[1]);

console.log(a+b);
 ```
 <img width="640" alt="스크린샷 2022-01-12 오후 12 54 58" src="https://user-images.githubusercontent.com/77538818/149061190-7039e36e-78ed-466d-813b-a78c105aec33.png"> 
 
 
 ### (3) 백준에 업로드 할 코드 : 1001.js
 index.js 실행 후 통과가 되면 github에 올리기 위해 백준에 업로드할 코드를 만들어 문제마다 파일을 따로 만들었다.   
 실제 백준 사이트에 코드 제출시에는 readFileSync() 의 파일명을 `/dev/stdin`으로 변경해야 한다!
 
 ```node.js
let input = require('fs').readFileSync('/dev/stdin').toString().split(' ');
var a = parseInt(input[0]);
var b = parseInt(input[1]);

console.log(a+b);
 ```

## 4. 백준 문제의 예제 입력 받기
### (1) 한 줄에 공백으로 값이 들어올 때
`1 2`로 들어올 때
 ```node.js
let input = require('fs').readFileSync('/dev/stdin').toString().split(' ');
var a = parseInt(input[0]);
var b = parseInt(input[1]);

console.log(a+b);
 ```
 input 변수에 공백으로 `split`한 문자들이 array 형태로 들어온다. `parseInt`를 통해서 하나씩 분리한다.
 
 ### (2) 한 줄에 하나씩 값이 들어올 때
 `1`   
 `2`   
 처럼 개행을 기준으로 값이 하나씩 들어올 때
  ```node.js
let input = require('fs').readFileSync('/dev/stdin').toString().split('\n');
var a = parseInt(input[0]);
var b = parseInt(input[1]);

console.log(a+b);
 ```
 `\n`개행문자로 `split`한다.
 
 
