# OAuth 개념 정리



- OAuth란?
    - 인증을 의한 오픈 스탠다드 프로토콜 → 사용자가 페이스북이나 트위터 같은 인터넷 서비스 기능을 다른 애플리케이션 (데스크톱, 웹 등)에서도 사용할 수 있게 한 것 (인증보다는 `허가` 목적이 크다)
    - 쉽게 설명한다면? (나방문, 김목적씨)
        1. 나방문(외부손님)가 업무 목적으로 김목적(회사 사원)을 만나서 회사에 왔다 
        2. 안내 데스크에서는 김목적씨에게 나방문씨가 방문했다고 연락한다
        3. 김목적씨가 안내 데스크로 찾아와 나방문씨의 신원을 확인해줌
        4. 김목적씨는 업무 목적과 인적 사항을 안내 데스크에서 기록
        5. 안내 데스크에서 나방문 씨에게 방문증을 발급
        6. 김목적씨와 나방문씨는 정해진 장소로 이동해 업무를 진행
        
        → '방문증'을 가진 사람은 정해진 곳만 갈 수 있듯이 직접 서비스에 로그인한  사용자와 OAuth를 이용해 권한을 인증받은 사용자는 할 수 있는 일이 다른 셈
        
        | 용어 | 설명 |
        | --- | --- |
        | Resource Owner | Resource Server에 계정을 가지고 있으면서, Client를 이용하려는 사용자 |
        | Resource Server | OAuth를 사용하는 Open API를 제공하는 서비스 (구글, 다음) |
        | Client | OAuth 인증을 사용해 Service Provider의 기능을 사용하려는 애플리케이션이나 웹 서비스 (내 서비스) |
        | Request Token | Client가 Resource Server에게 접근 권한을 인증받기 위해 사용하는 값, 인증이 완료된 후에는 Access Token으로 교환 |
        | Access Token | 인증 후 Client가 Resource Server의 자원에 접근하기 위한 키를 포함한 값 |
- Access Token이란?
    - 임의의 문자열 값으로 문자열 정체는 이 토큰을 발급해준 서비스만 알 수 있다
    - 이 토큰값과 관련된 고객의 정보를 우리는 해당 서비스에 요청할 수 있음
    - 해당 서비스는 이 토큰을 검증하고, 발급된게 맞다면 해당 고객의 정보를 넘겨줌
    - 즉, Access Token의 존재 자체가 `고객이 정보를 넘겨주는 것을 동의함`의 징표인셈
    
    Q) 우리는 Access Token을 어떻게 네이버로부터 받지? 
    
    - 당연히 네이버가 건네줘야함
    
    Q) 네이버는 어떻게 특정 고객의 Access Token을 발급해줄까?
    
    - 특정 고객이 네이버 회원임을 인증하면 된다
    
    Q) 고객 정보 확인 후 Access Token을 우리 서비스에서 어떻게 받을까?
    
    - `Redict` 서버가 클라이언트 보고 어디로 가라고 지정해주는 것 (우리가 네이버에게 알려줘야해)
    - 고객이 로그인 후 네이버에서 다시 우리 서비스로 리다이렉트 해주면 고객은 우리 서비스로 바로 넘어옴
    - URL에 묶어서 건네주면 된다!
    
    Q) 토큰을 받긴 받았는데... 이걸로 어떻게 회원 정보를 얻지?
    
    - 네이버에게 토큰 건네주면, 토큰 확인 후 네이버는 정보를 전달해줌
        
<img width="735" alt="스크린샷_2021-12-14_오전_10 47 01" src="https://user-images.githubusercontent.com/77538818/146699661-a10f80d6-74a0-4d22-abc5-fa1d90aef438.png">        
    
- OAuth 정리
    1. 서비스를 등록하는 과정
        - 구글에서 자사 서비스 등록
        - 이 과정에서 redirect_uri 등을 합의 (client id, client secret 부여받고 client는 따로 저장)
    2. 토큰 발급 및 사용 과정
        - Resource Owner는 Client 서비스에 접속하여 구글 로그인 시도 → 자동으로 Resource Server에 접속
        - 로그인 후 Resource Owner 화면에서 Client가 당신의 구글 정보를 사용하려 한다는 문구가 뜨며 승인 요청 → 사용자는 승인
        - Resource Server는 Client가 해당 사용자 구글 정보에 접근할 수 있도록 승인이 되었다는 code를 redirect url을 사용하여 Client에게 전송
        - 그 후 Client는 [client key, client id (어떤 서비스인지)], code(특정 사용자 정보 접근할 수 있다는 승인) 총 3가지 정보를 Resource Server에게 전달
        - 3가지 정보가 다 맞다면 access token, refresh token 발급해 Client에게 전달되고 서버에 저장해서 사용
        
        - 이후 access token 만료되면 refresh token으로 access token 재발급
            
<img width="875" alt="스크린샷_2021-12-15_오후_2 52 58" src="https://user-images.githubusercontent.com/77538818/146699703-006ec3a8-9297-4f7a-90f2-6f8bd84ef0a1.png">
            
        
 - Google OAuth scope
    - `scope` 액세스하려는 사용자 계정 부분을 나타내는 범위 목록
 - code와 access token의 역할이 비슷한 것 같은데..번거롭게 왜 사용하지?
    - access token을 얻기 위한 인증 코드를 교환하는 단계가 더 있어 보안에 효과적
