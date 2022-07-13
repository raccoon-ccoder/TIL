## Github 계정 여러개 사용하기
하나의 기기에서 여러 개의 깃허브 계정을 쓰고 싶어 계정 여러 개 사용하는 방법을 작성한 글이다.

<br />

순서는 다음과 같다.      
  1. ssh 계정 생성     
  2. ssh config 설정     
  3. ssh agent에 등록     
  4. github에 public key 등록    
  5. git clone 받기        
    
<br />

### 1. ssh key 생성
먼저 ssh라는 이름을 가진 숨김 폴더로 이동한다. 위치는 홈 디렉토리(~)에 위치해 있으며 존재하지 않는다면 생성 후 이동한다.    
이동하였다면 ssh-key를 생성한다.
```js
cd ~/.ssh
// 만약 ssh 폴더가 존재하지 않으면 $ mkdir ~/.ssh 먼저 실행 후 진행

ssh-keygen -t [암호화 방식] -b [생성할 key의 크기] -C "github계정 등록 메일 주소"
// ssh-keygen -t rsa -b 4096 -C "jy.baek@회사이메일"
```

<br />

위 명령어를 실행하면 public/private rsa key pair를 생성 중이라는 내용이 나온 후 생성할 파일의 이름을 적어달라 한다.     
이때 그냥 넘기지 않고 개인 계정과 회사 계정을 구분하기 위해 개인 계정은 `id_rsa_personal`, 회사 계정은 `id_rsa__회사명`로 하였다. 
```js
Generating public/private rsa key pair. // public/private rsa key pair를 생성 중

Enter file in which to save the key (/Users/baekjeongyeon/.ssh/id_rsa): // 이때 원하는 값으로 설정
```


<br />


이름 설정이 완료되면 해당 계정의 비밀번호를 입력하라 하는데 굳이 설정하지 않고 엔터로 넘겼다. (총 2번)
```js
Enter passphrase (empty for no passphrase): // 엔터 입력으로 넘어간다
Enter same passphrase again:  // 엔터 입력으로 넘어간다
```


<br />


그러면 아래와 같은 형태의 값이 출력되었다면 제대로 생성이 되었으며 ls 명령어를 통해 잘 생성되었는지 확인할 수 있다.     
참고로 .pub 파일은 공개, `.ssh`파일은 비공개 파일로 Github에 등록시 `.pub`파일을 활용해 등록한다.   
회사 계정도 생성했으므로 개인 계정도 동일하게 ssh-key를 생성한다.
```js
Your identification has been saved in id_rsa__회사명
Your public key has been saved in id_rsa__회사명.pub
The key fingerprint is:
SHA256:sha256sha256sha256sha256 jy.baek@회사이메일
The key's randomart image is:
+---[RSA 4096]----+
|       .+*SO=    |
|         ..oM=.  |
|        o. ==+ . |
|       . Jan. .  |
|      o G o      |
|   o o + o .     |
|  o o = * +      |
|   o.. = G .     |
|   oo .   d      |
+----[SHA256]-----+

$ ls
> id_rsa__회사명.pub	id_rsa__회사명
```


<br />


### 2. ssh config 설정
텍스트 편집기를 사용해 `config`파일을 생성하여 아래 내용을 입력한다.
```js
#  깃헙계정 work
#  -------------------
Host github.com-회사명
  HostName github.com
  IdentityFile ~/.ssh/id_rsa__회사명
  User jy.baek@회사이메일

# 깃헙계정 study
#  -------------------
Host github.com-personal
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_personal
  User qorwjddus96@naver.com
```
개인 계정, 회사 계정을 위한 2개의 새로운 host를 생성하였다.     
ssh 사용시 Host명을 저렇게 설정하면 해당하는 `HostName`으로 접속하고, `User`부분의 계정을 사용하며, 인증키로 `IdentityFile`를 참조하게 된다.    

<br />

### 3. ssh agent에 등록
생성된 key는 ssh agent에 등록해야 하며 아래와 같이 2개를 모두 등록한다.
```js
$ ssh-add [생성한 shh-key의 경로]
ssh-add ~/.ssh/id_rsa_personal
ssh-add ~/.ssh/id_rsa__회사명

// 완료 메세지
Identity added: /Users/baekjeongyeon/.ssh/id_rsa_personal (qorwjddus96@naver.com)

// 에러 메세지
Could not open a connection to your authentication agent.
// 해결 방법 (ssh-agent를 다시 실행)
eval $(ssh-agent)
```

<br/>

등록했다면 아래와 같이 agent에 등록된 내용을 다시 확인한다.
```js
$ ssh-add -l
4096 SHA256:hOc6am/EvnizF37cZ/SIoO6kcOeuA5TF/OncSOX5RU4 jy.baek@회사이메일 (RSA)
4096 SHA256:nN4YjUDNtkSmcxy4px2vQ71hjDFz0eFQLabq3FHwzDI qorwjddus96@naver.com (RSA)
```
> 참고 : 리붓 후에 ssh-add -l명령으로 확인해보면 아무것도 나오지 않지만 잘 작동한다.

<br/>

### 4. github에 public key 등록
먼저 터미널에서 아래와 같이 입력해 해당 문자들을 복사한다.     
```js
$ cat id_rsa__회사명.pub
```
github 계정 > settings > `SSH and GPG Keys`에서 새로운 SSH Key를 등록하는데 이때 title은 원하는 것을 작성하되 key에는 아까 복사한 값들을 넣어준다. 
참고로 key는 반드시 `.pub`확장자가 붙어 있는 파일의 내용 전체를 복사해 넣고 `ssh-rsa`로 시작해 직접 설정한 `이메일`로 끝난다.

<br/ >

### 5. git clone 받기
clone 받을 때 프로토콜을 HTTPS에서 SSH로 변경하고 복사한다.
터미널을 열어서 소스를 클론 받는데 기본 경로가 `git@github.com:[user]/[저장소]` 이렇게 표시된다.     
여기서 `git@guthub.com` 부분을 config 파일 만들 때 정해둔 Host명인 `github.com-회사명`으로 변경한다.    
```js
$ git clone git@github.com-회사명:companyRepo/sample.git
```
위와 같이 하면 정상적으로 다운로드된다.

### 6. 설정이 제대로 되었는지 확인하는 방법
```js
$ ssh -T git@github.com-[3번에서 github.com-(지정값) 여기서 지정한 지정값]

// 예시
$ ssh -T git@github.com-somjang
$ ssh -T git@github.com-work

The authenticity of host 'github.com (15.164.81.167)' can't be established.
RSA key fingerprint is SHA256:sha256sha256sha256sha256sha256sha256sha256
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
위처럼 명령어 실행시 연결하겠냐는 질문이 나오는데 yes 입력 후 아래와 같은 내용이 나온다면 설정이 완료되었습니다.
```js
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
Hi SOMJANG! You've successfully authenticated, but GitHub does not provide shell access.
```

<br />

### 7. 콘솔에서 계정 변경
```js
git config user.name 계정아이디
git config user.email 이메일

// 확인
git config user.name
git config user.email
```
변경 후 커밋하면 깃허브에 변경된 계정으로 커밋되는 것을 확인할 수 있다.
























