## [JavaScript] DOMException: play() failed because the user didn't interact with the document first
리액트, 타입스크립트 프로젝트에서 자동 재생 오디오 플레이어 구현 중 만난 에러다.    


### 원인
[정책 이슈](https://developer.chrome.com/blog/autoplay/)로 인해 발생한 에러로 자동 재생은 사용자의 인터렉션을 무시한 채, 벌어지는 상황으로 받아들일 수 있다.      
예를 들어 자동 재생으로 인해 원치 않는 소리가 나와 관련된 피해를 보는 상황이 발생할 수도 있다는 것이다.    

### 해결
현재 대부분 SNS를 보면 자동 재생은 하지만, 음소거인 상태를 확인할 수 있다. 이것도 자동 재생을 허용하는 정책 중 하나이다.     
하지만 굳이 음소건인  자동 재생을 실행하기 보다는 애초에 자동 재생을 하지 않는게 낫다는 생각에 사용자가 재생 버튼을 클릭해야만 음악을 재생하게끔 변경하였다.


#### 변경 전 코드
```js
function MusicPlayer() {
  const [music, setMusic] = useState(musics); // 음악 목록
  const [isPlayingMusic, setIsPlayingMusic] = useState(false);  // 음악이 재생되고 있는지 여부
  const [index, setIndex] = useState(0);  // 음약 번호
  const musicPlayer = useRef<HTMLAudioElement>(null); // audio ref

  const playMusic = () => {
    musicPlayer.current?.play();
    setIsPlayingMusic(true);
  };

  const pauseMusic = () => {
    musicPlayer.current?.pause();
    setIsPlayingMusic(false);
  };

  const loadMusic = () => { // 자동 재생 
    if (musicPlayer && musicPlayer.current && musicName.current) {
      musicPlayer.current.src = require("./" + music[index].src);
      musicPlayer.current.load();
      playMusic();
    }
  };

  const changeVolume = (e: React.FormEvent<HTMLInputElement>) => {
    const volume = e.currentTarget.value;
    if (musicPlayer && musicPlayer.current) {
      musicPlayer.current.volume = Number(volume) / 100;
    }
  };

  const changeNextMusic = () => {
    index < music.length - 1 ? setIndex((idx) => idx + 1) : setIndex(0);
  };

  const changePreviousMusic = () => {
    index > 0 ? setIndex((idx) => idx - 1) : setIndex(music.length - 1);
  };

  useEffect(() => {
    loadMusic();
    if (musicPlayer && musicPlayer.current) {
      musicPlayer.current.addEventListener("ended", () => changeNextMusic());
    }
    return () => {
      musicPlayer.current?.removeEventListener("ended", () =>
        changeNextMusic()
      );
    };
  }, []);

  useEffect(() => {
    loadMusic();
  }, [index]);

  return (
      ... 중략
        <S.MusicPlayer ref={musicPlayer} />
        <S.MusicIcons>
          <S.PreviousBtn onClick={changePreviousMusic} />
          {isPlayingMusic ? (
            <S.PauseBtn onClick={pauseMusic} />
          ) : (
            <S.PlayBtn onClick={playMusic} />
          )}
          <S.NextBtn onClick={changeNextMusic} />
       ...중략
  );
}
```
- `useEffect`를 이용해 처음 렌더링 될 때, 음악을 로드하는 `loadMusic`함수를 실행한다.
- 이전곡 버튼이나, 다음곡으로 넘어가는 버튼 클릭시 changeNextMusic, changePreviousMusic함수를 이용해 음악의 `index`를 변경하고 `useEffect`를 이용해 `index`변경시 `loadMusic`함수를 실행해 다음 노래를 재생한다.
- **자동 재생 기능을 제거하기 위해 처음 렌더링시에는 음악을 로드만 하며, index가 바뀔 때는 음악 로드 및 재생이 되게끔 구현하고 싶었다.**
- 여기서 index의 변화에 따른 `useEffect`훅은 update뿐만 아니라 `mount`, 즉 첫 렌더링시에도 발생되기에 계속 자동재생이 일어나는 문제가 발생한다.

### 변경 후 코드
```js
function MusicPlayer() {
  const [music, setMusic] = useState(musics);
  const [isPlayingMusic, setIsPlayingMusic] = useState(false);
  const [index, setIndex] = useState(0);

  const musicPlayer = useRef<HTMLAudioElement>(null);
  const mounted = useRef(false);  // 추가

  const playMusic = () => {
    musicPlayer.current?.play();
    setIsPlayingMusic(true);
  };

  const pauseMusic = () => {
    musicPlayer.current?.pause();
    setIsPlayingMusic(false);
  };

  const loadMusic = () => { // playMusic() 분리
    if (musicPlayer && musicPlayer.current && musicName.current) {
      musicPlayer.current.src = require("./" + music[index].src);
      musicPlayer.current.load();
    }
  };

  const changeVolume = (e: React.FormEvent<HTMLInputElement>) => {
    const volume = e.currentTarget.value;
    if (musicPlayer && musicPlayer.current) {
      musicPlayer.current.volume = Number(volume) / 100;
    }
  };

  const changeNextMusic = () => {
    index < music.length - 1 ? setIndex((idx) => idx + 1) : setIndex(0);
  };

  const changePreviousMusic = () => {
    index > 0 ? setIndex((idx) => idx - 1) : setIndex(music.length - 1);
  };

  useEffect(() => {
    loadMusic(); // 음악을 재생하지 않고 로드만 한다
    if (musicPlayer && musicPlayer.current) { // 음악 볼륨 0으로 조정
      musicPlayer.current.volume = 0;
    }
    if (musicPlayer && musicPlayer.current) {
      musicPlayer.current.addEventListener("ended", () => changeNextMusic());
    }
    return () => {
      musicPlayer.current?.removeEventListener("ended", () =>
        changeNextMusic()
      );
    };
  }, []);

// 추가
  useEffect(() => {
    if (mounted.current) {  
      loadMusic();
      playMusic();
    } else {
      mounted.current = true;
    }
  }, [index]);
 ... 중략 
}
```
- 자동 재생 기능을 제거하기 위해 loadMusic함수 내부에 음악을 재생하는 playMusic함수를 제거했다.
- index가 변경되는 경우에만 음악을 로드하고 재생하고 싶어서 useEffect 내부에 `useRef`객체를 활용했다.
- useEffect는 mount될때도 실행되는데 그때는 mounted가 false이므로 if문이 동작하지 않지만 else문에서 true로 변경했기에 다음 리렌더링 때부터는 if문 내부가 실행괸다.
- useRef는 그 안의 데이터가 변경되어도 화면을 리렌더링하지는 않지만, 리렌더링 후에도 데이터가 유지되기에 위와 같은 방법을 사용했다.


