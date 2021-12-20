# JWT 정리

- Cookie란?
    - 클라이언트가 웹사이트 접속시 사이트가 사용하게 되는 일련의 작은 기록파일
    - 서버가 클라이언트에 정보 전달시 저장하고자 하는 정보는 응답 헤더(Cookie)에 저장하여 전달
    - key - value 형식의 문자열 형태로 저장
        - 쿠키 동작 방식
        1. 클라이언트가 로그인 요청
        2. 서버에서 쿠키 생성 후 클라이언트로 전달
        3. 클라이언트가 서버로 요청을 보낼 때 쿠키를 전송
        4. 쿠키를 이용해 유저 인증을 진행
    - 단점
        - 민감 정보가 다 노출되어 보안적으로 위험
        - 웹 브라우저마다 쿠키에 대한 지원 형태가 달라 다른 브라우저간의 공유 불가
        - 서버는 매번 id,pw 받아서 인증해야하는 불편함 존재
- Session이란?
    - 쿠키를 기반으로 하지만 클라이언트가 아닌  서버에 저장하여 관리
    - http 장점인 stateless 위배 → 서버의 세션 저장소에 상태를 저장하는 상황이기 때문 (stateful)
    - 클라이언트 구별을 위해 세션 ID를 클라이언트마다 부여하며 클라이언트 종료 전까지 유지
        - 세션 동작 방식
        1. 클라이언트가 id, pw로 서버에 로그인 요청
        2. id, pw로 인증 후 사용자를 식별할 특정 유니크한 세션 ID를 만들어 자물쇠처럼 서버의 세션 저장소에 저장
        3. 세션 ID를 특정한 형태(쿠키 or json)로 클라이언트에게 다시 반환
        4. 이후 사용자 인증이 필요한 정보를 요청할 때마다 이 세션 ID를 쿠키에 담아 서버에 함께 전달
        
    - 단점
        - 세션 저장소의 문제가 발생하면 인증 체계가 무너져 이전에 인증된 유저 또한 인증 불가
        - 사용자 많아질수록 메모리를 많이 차지
        - 매번 요청시 세션 저장소를 조회해야 하는 단점
        
    
- JWT란?
    - Json Web Token으로 `인증에 필요한 정보들을 Token에 담아 암호화시켜 사용하는 토큰` (인코딩된 긴 텍스트 문자열)
    - 쿠키와 다른 점은 '서명된 토큰', 공개/개인 키를 쌍으로 사용해 토큰에 서명할 경우 서명된 토큰은 개인키를 보유한 서버가 이 서명된 토큰이 정상적인 토큰인지 인증할 수 있음
    - 세션 방식처럼 토큰 자체를 쿠키에 담아서 보낼수도 있고 HTTP 헤더에 담아서 보낼 수도 있다. (HTTP Only 옵션의 쿠키에 저장하는 것이 좋음)
    - 왜 사용하는걸까? 세션으로 로그인 처리를 하면 안되는걸까?
    - REST API 서버는 `stateless` 이기에 세션 정보나 쿠키를 별도로 저장하거나 관리하지 않고 들어오는 요청에 대해서만 단순히 수행하기에 세션을 통해 처리를 한다면 restful하다고 볼 수 없다.
    - 토큰 3가지 요소 (header.payload.signature)
        - `Header`  보통 토큰 타입이나 서명 생성에 어떤 알고리즘이 사용되었는지 저장
        
        ```sql
        { 
        	"alg": "HS256", 
        	"typ": JWT 
        }
        
        typ : 토큰의 타입을 지정 
        alg : 알고리즘 방식 지정, 서버에서 토큰 검증시 서명(Sgnature)에서 사용
        이 JSON은 최종적으로 Base64 URL로 인코딩
        -> eyJraWQiOiJ -TRUNCATED- JTMjU2In0
        ```
        
        - `Payload` 클라이언트에 대한 정보(JWT의 클레임들) 가장 중요!
        - Claim - 토큰에 담긴 사용자 정보 등의 데이터
        
        ```sql
        { 
        	[...] 
        	"iss": "https://cognito-idp.eu-west-1.amazonaws.com/XXX", 
        	"name": "Mariano Calandra", 
        	"admin": false 
        }
        
        iss : 등록된 클레임, 토큰을 발급한 자격 증명 공급자 (이 경우 amazon cognito)
        필요에 따라 클레임을 추가할 수 있음 (ex : admin 클레임)
        이 JSON은 최종적으로 Base64 URL로 인코딩
        -> eyJzdWIiOiJkZGU5N2Y0ZC0wNmQyLTQwZjEtYWJkNi0xZWRhODM1YzExM2UiLCJhdWQiOiI3c2Jzamh -TRUNCATED- hbnRfaWQiOiJ4cGVwcGVycy5jb20iLCJleHAiOjE1N jY4MzQwMDgsImlhdCI6MTU2NjgzMDQwOH0
        ```
        
        - `Signature` 인코딩된 헤더와 인코딩된 페이로드를 점(.)으로 연결하여 합친 후 비밀키로 해싱 (암호화하는 작업)
            - 헤더의 alg 속성(이 경우는 RS256)과 개인키에 지정된 암호화 알고리즘을 사용해 결괏값을 해시
            - 해시된 결과를 Base64 URL로 인코딩
            
            ```sql
            data = base64UrlEncode(header) + "." + base64UrlEncode(payload); 
            hash = RS256(data, private_key); 
            signature = base64UrlEncode(hash);
            -> POstGetfAytaZS82wHcjoTyoqhMyxXiWdR7Nn7A29DNSl0EiXLdwJ6xC6AfgZWF1bOsS_TuYI3OG85 -TRUNCATED- FfEbLxtF2pZS6YC1aSfLQxeNe8djT9YjpvRZA
            ```
            

- JWT를 HTTP header에 추가하여 서버에 요청을 하면 서버에서는 이를 디코딩하여 사용자 인증 거침
- 토큰 통작 방식
    1. 클라이언트가 로그인 요청
    2. 서버에서 유저의 고유한 ID와 다른 인증 정보들과 함께 Payload에 담음
    3. JWT의 유효기간 설정 및 옵션을 설정해줌
    4. Secret key를 이용해 토큰을 발급
    5. 발급된 토큰은 클라이언트에 쿠키 혹은 로컬 스토리지 등에 저장하여 요청을 보낼 때마다 같이 보냄
    6. 서버는 토큰을 Secret key로 헤더, 페이로드를 복호화하여 3번 서명값과 일치하는 결과가 나오는지 검증하는 과정 거침
    7. 검증 완료시 대응하는 데이터 보냄
- 토큰의 장점?
    - 세션보다 간편하여 세션처럼 별도 저장소 필요 X, 토큰 발급 후 클라이언트에게 전송 후 검증하는 과정만 있으면 됨.
- 토큰의 단점?
    - 발급된 JWT는 삭제가 불가하기에 토큰 탈취시, 유효 시간 종료되기 전까지 탈취자가 악용 가능
    - → 따라서 Refresh Token이라는 것을 이용해 피해를 줄일 수 있음
        - Refresh 토큰 방법이란?
        - 짧은 수명의 access 토큰, 보통 2주정도로 잡힌 refresh 토큰으로 2개를 발급
        - refresh 토큰은, 상응값을 DB에도 저장
        - 손님은 access 토큰 수명 다하면 refresh 토큰 보냄
        - 서버는 refresh 토큰을 DB에 저장된 값과 대조해보고 맞다면 새로운 access 토큰 발급 (로그인할 필요 X)
