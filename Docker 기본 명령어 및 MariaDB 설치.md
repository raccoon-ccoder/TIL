# Docker 기본 및 mariaDB 설치

- 1. **Docker Desktop 설치 for Apple silicon**
    
    [https://docs.docker.com/desktop/mac/apple-silicon/](https://docs.docker.com/desktop/mac/apple-silicon/)
    
- 2.  docker 기본 명령어 정리 -  [docker 레퍼런스](https://docs.docker.com/engine/reference/run/)
    
    ```sql
    
    # 컨테이너 생성과 동시에 컨테이너로 접속
    # 단순히 image 안의 파일을 실행할 목적으로 생성된 것이기에
    # 메인으로 실행되는 파일이 종료되면 컨테이너도 같이 종료된다
    # 따라서 계속해서 컨테이너를 유지하고 싶다면 -d 옵션을 이용!
    docker run [옵션][이미지명 or 이미지ID][실행할 파일]
    
    # 실행중인 컨테이너 목록 조회
    # 중단된 컨테이너 목록까지 보고 싶다면 -a
    docker ps 
    
    # 로컬 레지스트리에 저장된 이미지 목록
    docker images
    
    # 컨테이너 혹은 이미지 제거
    # 파일 시스템 변경 사항이 완전히 소실되며, docker ps로도 확인할 수 없음
    docker rm 컨테이너명
    docker rm -v 컨테이명
    docker rmi 이미지명
    
    # 이미 실행중인 컨테이너에서 command 실행
    # 이미 실행중인 컨테이너에서 /bin/bash 커맨드를 수행
    # 결과적으로 컨테이너의 쉘을 습득하며 command의 종료는 실행중인 컨테이너에 영향 X
    docker exec [options] container command
    ex) docker exec -it f72ca548214e /bin/bash
    
    # 컨테이너 시작, 재시작, 중지
    docker start/restart/stop [container name]
    
    # 진입했던 컨테이너 종료
    # 해당 명령어 사용시 컨테이너 빠져나오면서 동시에 종료되기에 서비스 종료될 수 있으므로 주의
    # 컨테이너 종료 없이 빠져나오려면 ctrl + P, Q 입력
    exit
    ```
    
    ```sql
    ## docker 자주 사용하는 run 옵션
    
    # 이 옵션을 줘야 컨테이너 안에서 터미널 실행 가능 (interactive)
    # 컨테이너와 상효작용할 수 있으며 호스트 터미널 표준 입력과 출력이 컨테이너 연결된다는 의미
    -it
    
    # 컨테이너가 중단되면 제거
    -rm
    
    # 호스트 포트와 컨테이너 포트 맵핑
    -p 8080:80
    
    # 환경변수 설정 (env)
    -e SOME_ENV=test
    
    # 호스트의 볼륨, 컨테이너 볼륨 맵핑
    -volume="/host/path:/container/path"
    
    # 컨테이너가 detached 모드에서 실행되며, 실행 결과로 컨테이너 ID만을 출력
    # 즉 컨테이너가 백그라운드로 실행
    -d
    ```
    
- 3. docker에 mariaDB 컨테이너 설치
    1. docker - Mariadb 이미지 다운로드 받기
    
    ```xml
    docker pull mariadb
    ```
    
    1. Mariadb 컨테이너 만들고 실행 (이렇게 하면 도커 이미지 다운로드 안해도 자동으로 다운로드 받음)
    
    ```
    docker run --name mariadb \
    -p 3307:3306 \
    -e MYSQL_ROOT_PASSWORD=jybeak \
    -d mariadb:10.4 \
    --character-set-server=utf8mb4 \
    --collation-server=utf8mb4_general_ci \
    --skip-character-set-client-handshake 
    
    # 밑에 인코딩 관련 환경변수들은 이미지 이름 뒤에 넣어야 명령 작동
    # host os는 포트 하나에는 하나의 컨테이너만 연결할 수 있기에
    # mariadb도 기본 포트는 3306이나 mysql을 먼저 설치했기에 포트 충돌 방지를 위해 3307로 변경
    ```
    
    - 명령어 관련 옵션 설명
        
        ```sql
        —name : 만들어서 사용할 컨테이너명
        
        -d : 컨테이너를 백그라운드에서 실행
        
        -p : 호스트와 컨테이너간의 포트를 연결 (host-port:container-port)
        
        -e : 기타 환경설정 (enviroment)
        
        MYSQL_ROOT_PASSWORD : mariadb의 root 사용자 초기 비밀번호 설정
        
        mariadb : 컨터이너 만들때 사용할 이미지 이름
        
        # mariaDB는 기본 UTF8이 3byte로 구현된다.
        # 이모지의 경우 4byte이기에 utf8mb4(4byte)로 인코딩 설정 변경 필요
        
        # character-set-server 
        # 서버에서 사용하는 기본 문자 집합
        # character_set_server 가 변경 될 때 자동으로 주어진 문자 집합에 대한 기본 데이터 정렬로 설정
        
        # collation-server 
        # 서버에서 사용하는 기본 데이터 정렬로
        
        # skip-character-set-client-handshake 
        # 클라이언트에서 설정한 문자셋을 무시하고 character_set_server 값으로 설정
        
        # utf8mb3 (>= MariaDB 10.6 ), utf8(<= MariaDB 10.5 )
        # old_mode 시스템 변수 utf8mb4의 값을 변경하여 암시하도록 설정할 수 있습니다 .
        
        # charset 문자집합 collation 정렬
        # collation은 텍스트 데이터를 정렬할 때 사용
        
        # utf8_general_ci
        # 텍스트 정렬시 a 다음에 b가 나타나야 하는 일반적 정렬방식
        # 성능 우선시, 일반적인 경우 사용하는 정렬
        # ex ) ÀÁÅåāă 등의 문자가 없어 A 로 치환
        
        # utf8_unicode_ci
        # 위 경우에 대한 예외 없이 정상적으로 비교
        # 성능부분에서 느릴 수 있음 (특이한 악센트 문자 사용 국가 아니라면 별로)
        ```
        
        mariaDB 기본 포트 번호는 3306이지만 mysql을 이미 설치했기에 충돌문제로 3307로 설정?
        
    
    1. bash를 통해 mariadb 컨테이너 접속
    
    ```sql
    docker exec -it mariadb /bin/bash
    ```
    
    1. 루트 계정으로 db 접속 및 DB 생성, 유저 생성
    
    ```sql
    
    #mariadb 접속 (in mariadb container)
    mysql -u root -pjybeak
    
    -- DB 생성
    CREATE DATABASE sample;
    
    -- 유저 생성
    CREATE USER '[사용자 아이디]'@'%' IDENTIFIED BY '[비밀번호]';
    
    -- 유저 권한, 여기서 %는 외부접속, 로컬접속 다 가능케함
    GRANT ALL ON DBname.* TO 'test'@'%';
    
    -- 데이터 베이스 목록
    show databases
    
    -- 현재 접속된 DB의 table 목록
    show tables
    
    -- DB 접속
    use <DB_name>
    
    -- 현재 접속된 스키마 외 다른 스키마 Table 목록 조회 방법
    show tables from <schema_name>
    
    -- 특정 Table의 index 목록 조회
    show index from <Table_name>
    
    -- mariadb user 목록 확인
    select * from mysql.user
    ```
    
    1. 오픈 소스 테스트를 위한 GNU bash 버전 업그레이드 (최소 v5.0)
    
    ```bash
    # 컨테이너에 접속한 상태에서 처리
    cat /proc/version # linux 버전 확인
    
    # 업데이트 및 업그레이드 실행
    apt-get update && apt-get upgrade
    
    apt-get install vim
    
    vi /etc/mysql/my.cnf
    ```
    
    1. MariaDB Chracter Set 확인
    
    ```sql
    show variables like 'c%';
    ```
    
    1. 환경설정 파일 변경 (vi 편집기 이용, 만약 컨테이너 설치시 설정 따로 안했을경우)
    
    ```sql
    
    -- /etc/mysql/my.cnf 파일에 해당 내용 추가
    [mysqld]
    character-set-server=utf8mb4
    collation-server=utf8mb4_general_ci #운영환경 characterset
    skip-character-set-client-handshake
    ```
    
    1. 서비스 재실행 및 재접속 (7번 과정을 진행했을 경우)
    
    ```xml
    docker stop mariadb
    docker start mariadb
    docker exec -it mariadb /bin/bash
    mysql -u root -pjybeak
    ```
    
    1. character set 다시 확인
    
    ```xml
    
    show variables like 'c%';
    ```
    
    ![스크린샷_2021-12-10_오전_12 08 09](https://user-images.githubusercontent.com/77538818/146704001-307d0fe9-d80b-4fe4-bac5-b953ba8e32c8.png)

- 4. docker tutorial
    
    **(1) 가상화란?**
    
    - 물리 하드웨어 시스템에서 여러 시뮬레이션 환경이나 전용 리소스를 생성하게 해주는 기술
    - 즉, 서버 자원을 나눠 가지며 성능을 분산하고 분산된 서버는 각기 다른 기능을 수행함
    
    **(2) 가상화 등장 배경?**
    
    - 예전에는 하나의 컴퓨터에 하나의 app만 운영할 수 있었지만 비효율적이기에 가상화가 등장
    - 등장으로 인해 여러 App 들이 가상 머신 위에 올라가고 여러 컴퓨터들을 띄울 수 있는 환경이 되어 1개의 컴퓨터에서 여러 환경을 가진 App 운영 가능
        - 근데 어떻게 다른 OS를 사용할 수 있을까?
        - 각 OS마다 리소스 관리,명령어를 해석하는 **'커널'**이 있는데 OS마다 규칙이 다 다르기에 **하이퍼 바이저**가 각 OS의 명령어를 하나의 명령어로 번역해준다.
        - **Hypervisor** - VM이 하드웨어 간의 IO 명령을 처리하는 인터페이스
            
            <img width="536" alt="스크린샷_2021-12-10_오후_7 06 42" src="https://user-images.githubusercontent.com/77538818/146704024-3a0ff777-1f6a-4046-86e1-c6d36fd78302.png">

    
    **(3) 가상 머신 (Virtual Machine)이란?**
    
    - 컴퓨터 상에 소프트웨어로 논리적으로 만들어낸 컴퓨터
    - 즉, 1대의 컴퓨터를 사용하더라도 마치 여러대를 사용하는 듯한 효과를 가지며 여러 운영체제(OS)를 동시에 다를 수 있는 가상 공간을 만들어주는 프로그램
    - 하지만 운영체제 위에 프로그램 실행을 위한 환경 설정을 해야하기에 자원, 시간 낭비...
    - 사용법이 간단하나 무겁고 느려...
    - 대표적으로 Virtual Box, Parallels 등...
    
    **(4) Container 등장**
    
    - VM 단점을 극복하기 위해 **격리된 공간에서 프로세스가 동작**하는 컨테이너 등장
    - 가상화 기술의 하나지만 별도의 OS나 드라이버 없이 Host OS를 공유하는 형태로 실행
    - VM이 서버를 여러 대로 사용할 수 있다면, 컨테이너는 **개별 애플리케이션을 위한 가상 공간 할당**
    - VM보다 작은 단위, 더 간단하고 빠르고, 효율적 애플리케이션 실행 가능!
    - 컨테이너는 **하나의 OS만 사용**하기에 여러 OS 사용이 가능한 VM보다는 용도 제한 있을 수 있음
    
    <img width="617" alt="스크린샷_2021-12-10_오후_7 20 47" src="https://user-images.githubusercontent.com/77538818/146704037-08fe446f-c7d4-475e-a676-d9103bb8e33b.png">

    [VM VS Container 차이점 비교](https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/about/containers-vs-vm)

    **(5) Docker란?**
    
    - 컨테이너 기반의 오픈소스 가상화 플랫폼
    - 리눅스 컨테이너 기술을 이용해 가상머신처럼 하드웨어 자원을 완전히 가상화 하는 것이 아닌 프로세스들만을 격리시켜 빠르게 애플리케이션 환경 구축 및 배포할 수 있게 해주는 기술
    - 기능별로 컨테이너를 만들어서 올리는데 만약 프로그램에서 에러 발생시, 코드 내림 → 수정 → 다시 올림 의 과정 없이 **컨테이너 하나만 수정**하고 다시 올리면 되기에 유지 보수가 좋음
    
    **(6) Docker 이미지란?**
    
    - 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것
    - 쉽게 말하면, 어떤 프로그램들을 이미지화해서 저장하는 셈
    - 컨테이너 상태 바뀌거나 컨테이너 삭제되어도 이미지는 변하지 않고 남아있음
    - Docker hub에 등록하거나 Docker Repository 저장소를 만들어 관리할 수 있음
        
         <img width="686" alt="스크린샷_2021-12-10_오후_7 45 33" src="https://user-images.githubusercontent.com/77538818/146704042-9db00fc7-40cf-4154-9a45-544ade16cf28.png">

    
    **(7) Docker 컨테이너란?**
    
    - 프로그램 및 프로그램 실행 환경을 담고 있는 패키지 이미지
    - 즉, 컨테이너라는 추상화된 이미지 안에 프로그램 실행에 필요한 환경,코드를 심고 도커 엔진으로 이미지들을 관리 → 하나의 운영체제에 여러 실행 환경 가질 수 있음
    
    **(8) Docker 동작원리**
    
    - 도커는 이미지를 통째로 생성X → 바뀐 부분만 생성 후 부모 이미지를 참조하는 방식으로 동작이기에 효율적으로 이미지 관리 (레이어 저장 방식)
    - 유니온 파일 시스템(UFS)을 이용해 여러 개의 레이어를 하나의 파일시스템으로 사용할 수 있게 해줌
    - 컨테이너 생성시도 기존 이미지 레이어 위에 read-write 레이어 추가되어 컨테이너 실행중에 생성하는 파일이나 변경된 내용은 읽기/쓰기 레이어에 저장되므로 여러개의 컨테이너를 생성해도 최소한의 용량만 사용
    - Docker Layer 설명
        - 이미지는 여러개의 읽기 전용 레이어로 구성되고 파일 추가 or 수정시 새로운 레이어 생성
        - ubuntu 이미지가 A+B+C의 집합이라고 가정
        - ubuntu이미지를 베이스로 만든 nginx 이미지는 A+B+C+nginx가 됨
        - nginx 이미지 베이스의 webapp 소스 - A+B+C+nginx+source
        - webapp 소스 수정시 - A+B+C+nginx+source(v2)
        
        ![스크린샷_2021-12-10_오후_11 59 04](https://user-images.githubusercontent.com/77538818/146704071-a07e9299-a54f-4923-80ca-51795d8c2565.png)

    - UFS란?
        - 여러 개의 파일 시스템을 하나의 파일 시스템에 마운트하는 기능
        - 여러 파일 시스템을 하나로 합칠 경우 중복되는 파일 없이 나중에 마운트된 파일로 덮어쓴다 (overlay)
        
    
    **(9) Docker Volume이란?**
    
    - 도커 컨테이너에 파일 저장시 default로 도커 컨테이너 Writable 레이어에 저장되는데, 컨테이너 레이어에 저장되기에 컨테이너가 내려가면 지워지는 임시 저장소다.
    - 컨테이너가 내려가도 컨테이너의 디스크를 영속적으로 저장,관리하는 것이Volume
    
    **(10) docker desktop 사용이유?**
    
    - 맥북 내에 컨테이너에 대한 정보를 빠르게 확인할 수 있음
    - 컨테이너 로그를 빠르게 제공
    - 컨테이너 라이프 사이클을 쉽게 핸들링 (stop, remove)
    
- 5. docker 삭제 후 재설치
    
    ```sql
    # 터미널에서 다음 명령어로 삭제 스크립트 파일을 다운로드 받아 실행
    
    curl -O https://raw.githubusercontent.com/docker/toolbox/master/osx/uninstall.sh
    chmod +x uninstall.sh
    sudo ./uninstall.sh
    
    # brew를 통해 설치했기에 패키지 삭제
    brew uninstall docker
    
    # CLI Command로 잔여 DOCKER 파일 삭제 (별도로 앱도 삭제)
    sudo rm -Rf /Applications/Docker.app
    sudo rm -f /usr/local/bin/docker
    sudo rm -f /usr/local/bin/docker-machine
    sudo rm -f /usr/local/bin/docker-compose
    sudo rm -f /usr/local/bin/docker-credential-desktop
    sudo rm -f /usr/local/bin/docker-credential-ecr-login
    sudo rm -f /usr/local/bin/docker-credential-osxkeychain
    sudo rm -Rf ~/.docker
    sudo rm -Rf ~/Library/Containers/com.docker.docker
    sudo rm -Rf ~/Library/Application\ Support/Docker\ Desktop
    sudo rm -Rf ~/Library/Group\ Containers/group.com.docker
    sudo rm -f ~/Library/HTTPStorages/com.docker.docker.binarycookies
    sudo rm -f /Library/PrivilegedHelperTools/com.docker.vmnetd
    sudo rm -f /Library/LaunchDaemons/com.docker.vmnetd.plist
    sudo rm -Rf ~/Library/Logs/Docker\ Desktop
    sudo rm -Rf /usr/local/lib/docker
    sudo rm -f ~/Library/Preferences/com.docker.docker.plist
    sudo rm -Rf ~/Library/Saved\ Application\ State/com.electron.docker-frontend.savedState
    sudo rm -f ~/Library/Preferences/com.electron.docker-frontend.plist
    
    # 방법 1. brew를 통해 재설치
    # Formulae는 CLI 환경, Casks는 GUI 환경이기에 초보일경우 GUI 환경이 더 나음
    brew install --cask docker
    
    # 방법2. 링크를 통해 설치
    [https://docs.docker.com/desktop/mac/install/](https://docs.docker.com/desktop/mac/install/)
    
    # docker.app 실행하고 컨테이너 실행
    docker run -d -p 80:80 docker/getting-started
    ```
