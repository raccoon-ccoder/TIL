## Object is possibly 'null'
리액트, 타입스크립트 기반 토이 프로젝트에서 audio element를 다루기 위해 `useRef`함수를 사용하다 만난 에러다.     
참고로 `useRef`함수는 `current`속성을 가지고 있는 객체를 반환하는데, 인자로 넘어온 초기값을 `current`속성에 할당한다.    
`current`속성은 값을 변경해도 상태를 변경할 때 처럼 React 컴포넌트가 다시 렌더링되지 않고 React 컴포넌트가 다시 렌더링되어도 `current`속성의 값이 유실되지 않는다.
```js
function MusicPlayer() {
  const musicPlayer = useRef(null);
  const [isPlayingMusic, setIsPlayingMusic] = useState(false);
  
  const playMusic = () => {
    musicPlayer.current.play(); // (*)
    setIsPlayingMusic(true);
  };

  const pauseMusic = () => {
    musicPlayer.current.pause(); // (*)
    setIsPlayingMusic(false);
  };
  

  const changeVolume = (e: React.FormEvent<HTMLInputElement>) => {
    const volume = e.currentTarget.value;
    musicPlayer.current.volume = Number(volume) / 100; // (*)
  };
  
  return (
      <S.MusicPlayer src={music1} ref={musicPlayer} />
      ... 중략
  );
}
```

### 해결 방법
일단 `useRef`의 제네릭에 타입을 설정한다. 위 코드에선 audio element기에 아래 코드로 변경했다. 
```js
const musicPlayer = useRef<HTMLAudioElement>(null);
```

그 후 컴포넌트 내에서 `useRef`의 반환값을 사용하기에 `null check`를 하거나 `옵셔널 체이닝`을 사용한다.    
초기값을 `null`로 하는 이유는 명시적으로 빈 레퍼런스를 생성했음을 알려주기 위함이다.    
changeVolume 함수에서 옵셔널 체이닝을 사용하지 않은 이유는 할당 표현식에서 왼쪽에는 optional property가 올 수 없기 때문이다.    
```js
function MusicPlayer() {
  const musicPlayer = useRef<HTMLAudioElement>(null); // (*)
  const [isPlayingMusic, setIsPlayingMusic] = useState(false);
  
  const playMusic = () => {
    musicPlayer.current?.play(); // (*)
    setIsPlayingMusic(true);
  };

  const pauseMusic = () => {
    musicPlayer.current?.pause(); // (*)
    setIsPlayingMusic(false);
  };

  const changeVolume = (e: React.FormEvent<HTMLInputElement>) => {
    const volume = e.currentTarget.value;
    if (musicPlayer && musicPlayer.current) { // (*)
      musicPlayer.current.volume = Number(volume) / 100;
    }
  };
  
  return (
      <S.MusicPlayer src={music1} ref={musicPlayer} />
      ... 중략
  );
}
```
