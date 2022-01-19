## 호출 스케줄링_setInterval, setTimeout
일정 시간이 지난 후에 원하는 함수를 예약 실행(호출) 할 수 있게 하는 것을 `호출 스케줄링`(scheduling a call)이라고 하며 `setInterval`과 `setTimeout` 메소드를 사용할 수 있는데 둘은 약간의 차이가 있다.

### setInterval(func|code, delay(ms))
- 일정한 시간 간격으로 함수를 `반복 실행`하는 메소드다.
- 1번째 argument로 실행하고 하는 함수 또는 코드, 2번째 argument는 호출되는 function을 몇 ms 간격으로 할지 설정한다.
```js
function getClock() {
    const date = new Date();
    const hours = String(date.getHours()).padStart(2,"0");
    const minutes = String(date.getMinutes()).padStart(2,"0");
    const seconds = String(date.getSeconds()).padStart(2,"0");

    clock.innerText = `${hours}:${minutes}:${seconds}`;
}

// 먼저 호출한 이유는 setInterval()는 100ms 뒤에 getClock() 메소드가 호출된다
// 따라서 화면 처음 접속하면 getClock()이 실행되지 않기에 바로 시간을 나타내기 위해 호출
getClock();
setInterval(getClock, 1000);
```
- `getClock()` 메소드로 현재 시간을 구하고 `setInterval()` 메소드를 통해 `1초 간격`으로 시간을 구하는 코드를 작성했다.
- 참고로 `setInterval()` 설정시 getClock() 메소드가 바로 실행되는 것이 아닌 1초 뒤에 처음 실행되며 그 후로 1초 간격으로 getClock()이 실행된다.
- 따라서 화면에 처음 접속했을 시 시간을 바로 출력하기 위해 getClock() 메소드를 위에 호출해주었다.

### setTimeout(func|code, delay(ms))
- 지연시간을 발생시킨 후 특정 함수나 코드를 `1번`만 호출하는 메소드다.
- 1번째 argument로 실행하고 하는 함수 또는 코드, 2번째 argument로 실행 전 대기 시간을 설정한다.
```js
function add(x, y) {
  console.log(x + y);
}
setTimeout(add, 2000, 3, 4);
```
- `2초 후`에 add() 메소드 결과인 7이 출력되는 것을 볼 수 있다.


### 결론
> 일정 시간 간격을 두고 계속 실행하고 싶다면 setInterval()   
> 일정 시간 이후 1번만 실행하고 싶다면 setTimeout()   
> setInterval(), setTimeout() 함수 사용 후 더이상 사용하지 않는다면 clearInterval(), clearTimeout() 함수를 통해 타이머를 청소해주는 것이 좋다. (SPA 개발시 이 부분이 메모리 누수로 이어질 수 있기 때문이다)
