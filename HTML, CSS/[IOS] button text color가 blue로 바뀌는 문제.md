## [IOS] button text color가 blue로 바뀌는 문제
studymanager 토이 프로젝트 진행 중 IOS에서 button의 text color가 blue로 바뀌는 일이 있었다.

### 디버깅
그래서 맥북에 아이폰 연결 후 사파리에서 디버깅을 해봤다.   

<br/>

![ios default color](https://user-images.githubusercontent.com/77538818/156947783-9ecc08d0-7787-4a3c-8c8e-7794e5e61269.png)       

-apple-system-blue로 인해 default color가 변경된듯하다.   
아무래도 default 값에 익숙해져 color 명시를 안하여 이런 문제가 생긴 것 같다.

### 해결
특정 요소에 color를 명시하거나 html의 color를 black으로 명시하여 해결한다.    
이번 프로젝트에선 해당 버튼에 color 값을 명시하였다.

### 마무리
웹은 어떤 환경이냐에 따라서 변수가 많이 생기므로 특정 환경의 default 값에 의존하지 않고 항상 정확히 명시하는 습관을 들이는 것이 좋을 듯 하다.
